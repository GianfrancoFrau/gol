<script setup lang="ts">
import { computed, onBeforeUnmount, onMounted, ref } from 'vue';
import GridCell from './GridCell.vue';

const cols = 100;
const rows = 50;

const grid = ref([] as any);
const running = ref(false);
const gen = ref(0);
const speed = ref(500);
const generationTimer = ref(0);
let population: any = {};

const getNeighbours = (x: number, y: number) => {
  const g = [...grid.value];
  let n = 0;

  // top
  if (g[x - 1] && g[x - 1][y] && g[x - 1][y].alive) n++;
  // top-right
  if (g[x - 1] && g[x - 1][y + 1] && g[x - 1][y + 1].alive) n++;
  // top-left
  if (g[x - 1] && g[x - 1][y - 1] && g[x - 1][y - 1].alive) n++;
  // left
  if (g[y - 1] && g[x][y - 1].alive) n++;
  // right
  if (g[y + 1] && g[x][y + 1].alive) n++;
  // bottom
  if (g[x + 1] && g[x + 1][y].alive) n++;
  // bottom-right
  if (g[x + 1] && g[x + 1][y + 1] && g[x + 1][y + 1].alive) n++;
  // bottom-right
  if (g[x + 1] && g[x + 1][y - 1] && g[x + 1][y - 1].alive) n++;

  return n;
}

/*
    1. Any live cell with fewer than two live neighbours dies, as if by underpopulation.
    2. Any live cell with two or three live neighbours lives on to the next generation.
    3. Any live cell with more than three live neighbours dies, as if by overpopulation.
    4. Any dead cell with exactly three live neighbours becomes a live cell, as if by reproduction.
*/
// x = row, y = col
const isAlive = (alive: boolean, x: number, y: number) => {

  const n = getNeighbours(x, y);

  if (alive && n < 2) {
    // should dead
    return false;
  }

  if (alive && (n === 2 || n === 3)) {
    // should live
    return true;
  }

  if (alive && n > 3) {
    // should dead
    return false;
  }

  if (!alive && n === 3) {
    // should live
    return true;
  }
}

const stopGeneration = () => {
  clearInterval(generationTimer.value);
  generationTimer.value = 0;
  console.log('stopGeneration', generationTimer.value);
}

const generate = () => {
  const g = [...grid.value]
  for (let x = 0; x < rows; x++) {
    g[x] = [...g[x]];
    for (let y = 0; y < cols; y++) {
      g[x][y] = { alive: isAlive(g[x][y].alive, x, y) }
    }
  }
  grid.value = g;
  gen.value = gen.value + 1;

  console.log('generation', gen.value);
}

const startGeneration = () => {

  generationTimer.value = setInterval(() => {
    generate();
  }, speed.value);
}

const resetGen = () => {
  gen.value = 0;
}

const resetGrid = (init: boolean = false) => {
  const _grid: any = [];
  for (let x = 0; x < rows; x++) {
    _grid[x] = [] as any;
    for (let y = 0; y < cols; y++) {
      const random = Math.random() >= .5;
      _grid[x][y] = { alive: init ? false : random }
    }
  }
  grid.value = _grid;
}

const onCellClick = (x: number, y: number) => {
  console.log('onCellClick', x, y, grid.value[x][y])
}

const stop = () => {
  stopGeneration();
  running.value = false;
}

const start = () => {
  resetGrid();
  startGeneration();
  running.value = true;

  console.log('start', running.value);
}

const clear = () => {
  if(generationTimer.value) {
    stop();
  }
  resetGrid(true);
  resetGen();
}

const random = () => {
  resetGrid();
  resetGen();
}

const listenToEvents = () => {
  window.addEventListener('gol:start', start);
  window.addEventListener('gol:random', random);
  window.addEventListener('gol:clear', clear);
  window.addEventListener('gol:stop', stop);
}

const stopListenToEvents = () => {
  window.removeEventListener('gol:start', start);
  window.removeEventListener('gol:random', random);
  window.removeEventListener('gol:clear', clear);
  window.removeEventListener('gol:stop', stop);
}

const init = () => {

  resetGrid(true);

  population = computed(() => {
    let a = 0;
    let d = 0;
    for (let x = 0; x < rows; x++) {
      for (let y = 0; y < cols; y++) {
        if (grid.value[x][y].alive) {
          a++;
        } else {
          d++;
        }
      }
    }
    return {
      alive: a,
      dead: d
    };
  })

  listenToEvents();
}

onMounted(init);
onBeforeUnmount(() => {
  stopListenToEvents();
});

defineExpose({
  running,
  start,
  stop
})

</script>

<template>
  <div class="gol-grid">
    <div class="gol-row" v-for="(row, x) in grid" :key="x">
      <GridCell v-for="(c, k) in row" :key="x + k" :index="x + k" :alive="c.alive" @click="() => onCellClick(x, k)" />
    </div>
  </div>
  <div class="gol-stats">
    <div class="box">
      STATS
    </div>
    <div class="box">
      GEN: {{ gen }}
    </div>
    <div class="box">
      ALIVE: {{ population.alive }}
    </div>
    <!-- <div class="box">
      DEAD: {{ population.dead }}
    </div> -->
  </div>
</template>

<style scoped lang="scss">
.gol-grid {
  display: flex;
  flex-wrap: wrap;
  border: 1px solid var(--color-border);
  border-radius: var(--border-radius);

  .gol-row {
    width: 100%;
    display: flex;
  }
}

.gol-stats {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.5rem 1rem;
  margin-top: 1rem;
  border: 1px solid var(--color-border);
  border-radius: var(--border-radius);

  .box {}
}
</style>
