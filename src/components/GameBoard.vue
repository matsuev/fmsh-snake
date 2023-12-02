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
import { onBeforeMount, onMounted, reactive, ref } from 'vue'

const Board = reactive([])
const Snake = reactive([])

const dCol = ref(0)
const dRow = ref(-1)

onBeforeMount(() =>
{
   InitBoard()
   InitSnake()
})

onMounted(() =>
{
   setInterval(() => RenderSnake(), 1000)
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
         seg: i == 0 ? 1 : 2
      })
   }
}

function RenderSnake()
{
   // Движение головы
   let Head = Snake[0]

   Head.col += dCol.value
   Head.Row += dRow.value

   Snake[0].seg = 2

   Snake.unshift(Head)

   // Хвост

   let Tail = Snake.pop()

   Snake.forEach((cell) => Board[cell.row][cell.col] = cell.seg)
   Board[Tail.row][Tail.Col] = 0
}

</script>

<style>
.board-cell {
   width: 20px;
   height: 20px;
   display: inline-block;
   border: 1px dotted #ccc;
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