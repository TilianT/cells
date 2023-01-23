<template>
  <h1>GRID {{ gridSize }}x{{ gridSize }}</h1>

  <div class="grid">
    <div
      class="row"
      v-for="(row, rowIndex) in grid"
      :key="rowIndex"
    >
      <Cell
        v-for="(cell, colIndex) in row"
        :key="rowIndex+colIndex"
        :data="cell"
        :row="rowIndex"
        :column="colIndex"
        :isFocus="(rowFocus === rowIndex || colIndex == colFocus) ? true : false"
        @updateCell="changeCell"
        @mouseover="handleMouse(colIndex, rowIndex)"
      />
    </div>
  </div>
</template>

<script>
import { reactive, watch, ref } from 'vue';
import Cell from '@/components/Cell';

export default {
  components: { Cell },
  setup() {
    const gridSize = 50;
    const grid = reactive(Array.from(Array(gridSize), () => new Array(gridSize).fill(0)));
    const fibonachiStr = '11235';
    const colFocus = ref(0);
    const rowFocus = ref(0);

    function changeCell({ row, column }) {
      for(let i = 0; i < gridSize; i++) {
        grid[row][i] += 1;
        const colVal = grid[i][column];
        grid[i][column] = row !== i ? (colVal + 1) : colVal;
      }
    }

    function handleMouse(col, row) {
      colFocus.value = col;
      rowFocus.value = row;
    }

    function findSubarray(arr) {
      const indices = [];
      const arrString = arr.toString().replace(/,/g, '');
      let idx = arrString.indexOf(fibonachiStr);

      while (idx != -1) {
        indices.push(idx);
        idx = arrString.indexOf(fibonachiStr, idx + 1);
      }
      return indices;
    }

    watch(grid, () => {
      setTimeout(() => {
        for(let x = 0; x < gridSize; x++) {
          const fromColumns = findSubarray(grid[x]);
          if(fromColumns.length > 0) {
            fromColumns.forEach(index => {
              for(let y = index; y < index + 5; y++) {
                grid[x][y] = 0;
              }
            })
          }

          let colArr = [];
          for(let y = 0; y < gridSize; y++) {
            colArr.push(grid[y][x]);
          }
          const fromRows = findSubarray(colArr);
          if(fromRows.length > 0) {
            fromRows.forEach(index => {
              for(let y = index; y < index + 5; y++) {
                grid[y][x] = 0;
              }
            })
          }
        }
      }, 0);
    });

    return {
      grid,
      gridSize,
      colFocus,
      rowFocus,
      handleMouse,
      changeCell,
    }
  }
}
</script>

<style lang="scss">
* {
  box-sizing: border-box;
}
#app {
  font-size: 16px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;

  color: #2c3e50;
}

h1 {
  margin: 0;

  font-size: 2rem;
}

.grid {
  display: flex;
  flex-wrap: wrap;
  border-radius: 8px;

  .row {
    display: flex;
    width: 100%;

    .column {
      width: calc(100%/v-bind(gridSize));
    }
  }
}
</style>
