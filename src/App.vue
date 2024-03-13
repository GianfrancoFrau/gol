<template>
  <header>
    <h1>{{ title }}</h1>

    <div class="buttons">
      <button class="btn" :disabled="started" @click="random()">
        <IconShuffle />
      </button>
      <button class="btn" v-if="canPlay" @click="start()">
        <IconPlay />
      </button>
      <button class="btn" v-if="canPause" @click="pause()">
        <IconPause />
      </button>
      <button class="btn" :disabled="!started" @click="stop()">
        <IconStop />
      </button>
      <button class="btn" @click="clear()">
        <IconReset />
      </button>
      <button class="btn" @click="openInfoDialog()">
        <IconInfoSquareFill />
      </button>
    </div>
  </header>

  <main>
    <GolGrid></GolGrid>
  </main>

  <InfoDialog :title="title" :open="infoDialogOpen"></InfoDialog>
</template>

<script setup lang="ts">

import IconStop from './components/icons/IconStop.vue';
import IconReset from './components/icons/IconReset.vue';
import IconShuffle from './components/icons/IconShuffle.vue';
import GolGrid from '@/components/GolGrid.vue';
import { computed, onMounted, onUpdated, ref } from 'vue';
import InfoDialog from './components/InfoDialog.vue';
import IconPlay from './components/icons/IconPlay.vue';
import IconPause from './components/icons/IconPause.vue';
import IconInfoSquareFill from './components/icons/IconInfoSquareFill.vue';

const title = ref('VUE GOL');
const infoDialogOpen = ref(false);
const started = ref(false);
const paused = ref(false);

const canPlay = computed(() => {
  return !started.value || paused.value;
})

const canPause = computed(() => {
  return started.value && !paused.value;
})

const openInfoDialog = () => {
  infoDialogOpen.value = true;
}

const closeInfoDialog = () => {
  infoDialogOpen.value = false;
}

const start = () => {
  console.log('APP start');
  window.dispatchEvent(new CustomEvent('gol:start', { detail: { resume: started.value } }));
  started.value = true;
  paused.value = false;
}

const random = () => {
  console.log('APP random');
  window.dispatchEvent(new CustomEvent('gol:random'));
}

const stop = () => {
  console.log('APP stop');
  window.dispatchEvent(new CustomEvent('gol:stop'));
  started.value = false;
  paused.value = false;
}

const pause = () => {
  console.log('APP pause');
  window.dispatchEvent(new CustomEvent('gol:pause'));
  paused.value = true;
}

const clear = () => {
  console.log('APP clear');
  window.dispatchEvent(new CustomEvent('gol:clear'));
  started.value = false;
  paused.value = false;
}


onMounted(() => {
  window.addEventListener('gol:close-dialog', () => closeInfoDialog());
});

onUpdated(() => {
  console.log('onUpdated', started.value, paused.value, canPlay.value, canPause.value);
})

</script>

<style scoped lang="scss">
header {
  display: flex;
  align-items: center;
  border: 1px solid var(--color-border);
  border-radius: var(--border-radius);
  margin: 20px 0;
  padding: 0.5rem 1rem;

  h1 {
    flex: 1 auto;
    margin: 0;
    padding: 0;
    font-size: 1.2rem;
    font-weight: bold;
  }

  .buttons {
    display: flex;

    .btn {
      padding: 1rem;
      display: flex;
      align-content: center;
      background-color: #00000025;
      color: var(--color-text);
      border: 1px solid var(--color-border);
      margin-right: 0.3rem;

      &:hover:not(:disabled) {
        background-color: #00000050;
      }

      &:disabled {
        opacity: 0.6;
      }

      svg {
        transform: scale(1.5);
      }

      &:last-child {
        margin-right: 0;
      }

      &:hover {
        cursor: pointer;
        border-color: var(--color-border-hover);
      }
    }
  }
}
</style>
