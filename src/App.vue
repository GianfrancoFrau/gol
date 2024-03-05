<template>
  <header>
    <h1>Game Of Life</h1>

    <div class="buttons">
      <button class="btn" :disabled="canStop" @click="start()">
        <IconShuffle />
      </button>
      <!-- <button class="btn" v-if="canStop" @click="pause()">
        <IconPause />
      </button> -->
      <button class="btn" :disabled="!canStop" @click="stop()">
        <IconStop />
      </button>
      <button class="btn" @click="clear()">
        clear
      </button>
      <button class="btn">
        info
      </button>
      <!-- <RouterLink to="info">INFO</RouterLink>
      <span>Change theme</span> -->
    </div>
  </header>

  <main>
    <GolGrid ref="grid"></GolGrid>
  </main>

  <!-- <RouterView /> -->
</template>

<script setup lang="ts">

// import { RouterView } from 'vue-router'
// import IconPlay from './components/icons/IconPlay.vue';
import IconStop from './components/icons/IconStop.vue';
import IconShuffle from './components/icons/IconShuffle.vue';
import GolGrid from '@/components/GolGrid.vue';
import { ref } from 'vue';
import IconPause from './components/icons/IconPause.vue';

const grid = ref(null);
const canStop = ref(false);

const start = () => {
  console.log('APP start');
  canStop.value = true;
  window.dispatchEvent(new CustomEvent('gol:start'));
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
  stop();
  window.dispatchEvent(new CustomEvent('gol:clear'));
}

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
