<template>
  <div class="backdrop" v-if="props.open">
    <dialog :open="props.open" :class="dialogCss">
      <button class="dialog-close" @click="closeDialog()">
        <IconCloseSquare />
      </button>
      <header>
        <h1>{{props.title}}</h1>
        <h2>A <a href="https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life" target="_blank">Game Of Life</a> implementation in <a href="https://vuejs.org/" target="_blank">vuejs 3</a></h2>
        <h3>Thanks to <a href="https://icons.getbootstrap.com/" target="_blank">Bootstrap Icons</a></h3>
        <p class="footer">
          <span class="version">version <b>{{ version }}</b></span>
          <a href="https://github.com/GianfrancoFrau/vue-gol" target="_blank">Source code</a>
        </p>
      </header>
    </dialog>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue';
import IconCloseSquare from './icons/IconCloseSquare.vue';

const props = defineProps({
  open: {
    type: Boolean,
    default: false
  },
  title: {
    type: String,
    default: ''
  }
});

const version = ref(APP_VERSION);

// TODO change on dialog close ?
const animation = ref('zoomIn')

const dialogCss = computed(() => ({
  animate__animated: true,
  [`animate__${animation.value}`]: true
}))

const closeDialog = () => {
  window.dispatchEvent(new CustomEvent('gol:close-dialog'));
}
</script>

<style scoped lang="scss">
.backdrop {
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  display: flex;
  background-color: #00000050;

  dialog {
    align-self: center;
    justify-self: center;
    overflow-y: scroll;
    background-color: #00000095;
    color: white;
    border: 0;
    border-radius: var(--border-radius);
    font-size: 1.4rem;
    padding: 2rem;

    .dialog-close {
      position: absolute;
      top: 1rem;
      right: 1rem;
      display: flex;
      align-content: center;
      border: none;
      background-color: transparent;
      cursor: pointer;

      svg {
        transform: scale(1.2);
        color: white;
      }
    }

    a {
      text-decoration: none;
      font-style: italic;
      color: white;
      padding-bottom: 0.2rem;
      border-bottom: 2px dashed white;
    }

    h1 {
      font-size: 1.6rem;
      margin-bottom: 1rem;
    }

    h2 {
      font-size: 1.2rem;
      margin-bottom: 1rem;
    }

    h3 {
      font-size: 1rem;
    }

    .footer {
      margin-top: 2rem;
      font-size: 0.8rem;
      display: flex;
      align-items: center;
      justify-content: space-between;

      span {
        
      }
    }
  }
}
</style>
