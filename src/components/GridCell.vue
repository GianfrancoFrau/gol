<script setup lang="ts">
import { onMounted } from 'vue';

defineProps({
  index: Number,
  alive: {
    type: Boolean,
    default: false
  }
})

</script>

<template>
  <div :class="{ 'grid-cell': true, 'alive': alive }">

  </div>
</template>

<style lang="scss">
.grid-cell {
  flex: 1 auto;
  min-width: 5px;
  display: flex;
  width: 10px;
  height: 10px;
  align-content: center;
  justify-content: center;
  transform: all 0.5s;
  border: 1px solid var(--color-border);

  &.alive,
  &:hover {
    background-color: var(--vt-c-white);
  }
}
</style>
