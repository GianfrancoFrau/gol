<template>
  <header>
    <h1>{{ title }}</h1>

    <div class="buttons">
      <button class="btn" :disabled="canStop" @click="random()">
        <IconShuffle />
      </button>
      <button class="btn" :disabled="canStop" @click="start()">
        <IconPlay />
      </button>
      <button class="btn" :disabled="!canStop" @click="stop()">
        <IconStop />
      </button>
      <button class="btn" @click="clear()">
        reset
      </button>
      <button class="btn" @click="openInfoDialog()">
        info
      </button>
    </div>
  </header>

  <main>
    <GolGrid ref="grid"></GolGrid>
  </main>

  <InfoDialog :open="infoDialogOpen"></InfoDialog>

  <!-- <RouterView /> -->
</template>

<script setup lang="ts">

// import { RouterView } from 'vue-router'
// import IconPlay from './components/icons/IconPlay.vue';
import IconStop from './components/icons/IconStop.vue';
import IconShuffle from './components/icons/IconShuffle.vue';
import GolGrid from '@/components/GolGrid.vue';
import { ref } from 'vue';
import InfoDialog from './components/InfoDialog.vue';
import IconPlay from './components/icons/IconPlay.vue';

const title = ref('GOL');
const grid = ref(null);
const infoDialogOpen = ref(false);
const canStop = ref(false);

const openInfoDialog = () => {
  infoDialogOpen.value = true;
}

const closeInfoDialog = () => {
  infoDialogOpen.value = false;
}

const start = () => {
  console.log('APP start');
  canStop.value = true;
  window.dispatchEvent(new CustomEvent('gol:start'));
}

const random = () => {
  console.log('APP random');
  window.dispatchEvent(new CustomEvent('gol:random'));
}

const stop = () => {
  console.log('APP stop');
  canStop.value = false;
  window.dispatchEvent(new CustomEvent('gol:stop'));
}

const pause = () => {
  console.log('APP pause');
  canStop.value = false;
  window.dispatchEvent(new CustomEvent('gol:pause'));
}

const clear = () => {
  canStop.value = false;
  window.dispatchEvent(new CustomEvent('gol:clear'));
}

window.addEventListener('gol:close-dialog', () => closeInfoDialog());

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
      padding: 0.5rem;
      display: flex;
      align-content: center;
      background-color: transparent;
      color: var(--color-text);
      border: 1px solid var(--color-border);
      margin-right: 0.3rem;

      &:disabled {
        opacity: 0.6;
      }

      svg {
        transform: scale(1.2);
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
