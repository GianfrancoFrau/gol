<template>
  <header>
    <h1>Game Of Life</h1>
    <!--
    <div class="buttons">
      <RouterLink to="info">INFO</RouterLink>
      <span>Change theme</span>
    </div>
    -->
  </header>

  <RouterView />
</template>

<script setup lang="ts">

/*

1. Any live cell with fewer than two live neighbours dies, as if by underpopulation.
2. Any live cell with two or three live neighbours lives on to the next generation.
3. Any live cell with more than three live neighbours dies, as if by overpopulation.
4. Any dead cell with exactly three live neighbours becomes a live cell, as if by reproduction.

*/


import { RouterView } from 'vue-router'
</script>

<style scoped lang="scss">
header {
  display: flex;
  align-items: center;
  border: 1px solid var(--color-border);
  border-radius: var(--border-radius);
  margin: 20px 0;
  padding: 0.5rem 1.5rem;

  h1 {
    flex: 1 auto;
    margin: 0;
    padding: 0;
    font-size: 1.2rem;
    font-weight: bold;
  }
}
</style>
