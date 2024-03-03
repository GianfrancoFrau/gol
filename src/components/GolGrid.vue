<script setup lang="ts">
import { onMounted, ref } from 'vue';
import GridCell from './GridCell.vue';

const grid = ref([] as any);

const init = () => {
  const cols = 100;
  const rows = 50;
  const _grid = [];

  for (let x = 0; x < rows; x++) {
    _grid[x] = [] as any;
    for (let y = 0; y < cols; y++) {
      _grid[x][y] = { alive: false }
    }
  }

  console.log('INIT. The Grid', _grid);

  grid.value = _grid;
}

const onMouseDown = (x: number, y: number) => {
  const newGrid: any = grid.value.map((r: any) => [...r]);
  newGrid[x][y] = { alive: true };
  grid.value = newGrid;

  console.log('onMouseDown', x, y, newGrid[x][y]);
}

onMounted(init);

</script>

<template>
  <div class="gol-grid">
    <div class="gol-row" v-for="(row, i) in grid" :key="i">
      <GridCell v-for="(c, j) in row" :key="i + j" :index="i + j" :alive="c.alive" @mousedown="onMouseDown(i, j)" />
    </div>
  </div>
</template>

<style scoped lang="scss">
.gol-grid {
  display: flex;
  flex-wrap: wrap;
  border: 1px solid var(--color-border);

  .gol-row {
    width: 100%;
    display: flex;
  }
}
</style>
