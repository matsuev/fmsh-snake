<template>
   <div class="game-board">
      <div class="board-row" v-for="(row, rowIdx) in Board" :key="rowIdx">
         <div class="board-cell" :class="{
            'snake-head': cell == 1,
            'snake-body': cell == 2,
            'target': cell == 3
         }" v-for="(cell, cellIdx) in row" :key="cellIdx">
            &nbsp;
         </div>
      </div>
   </div>
</template>

<script setup>
import { onBeforeMount, onMounted, reactive } from 'vue'

const Board = reactive([])
const Snake = reactive([])

onBeforeMount(() =>
{
   InitBoard()
   InitSnake()
})

onMounted(() =>
{
   RenderSnake()
})

function InitBoard()
{
   for (let y = 0; y < 25; y++)
   {
      let row = []
      for (let x = 0; x < 25; x++)
      {
         row.push(0)
      }
      Board.push(row)
   }
}

function InitSnake()
{
   for (let i = 0; i < 5; i++)
   {
      Snake.push({
         col: 12,
         row: 12 + i,
         seg: i == 0 ? 1 : 2,
      })
   }
}

function RenderSnake()
{
   Snake.forEach(segment => Board[segment.row][segment.col] = segment.seg)
}

</script>

<style>
.board-cell {
   width: 20px;
   height: 20px;
   text-align: center;
   line-height: 20px;
   display: inline-block;
   border-radius: 4px;
   margin: 1px;
}

.snake-head {
   background-color: red;
}

.snake-body {
   background-color: blue;
}

.target {
   background-color: green;
}
</style>