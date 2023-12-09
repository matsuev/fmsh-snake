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

const emit = defineEmits(['score'])

const Board = reactive([])
const Snake = reactive([])

const dCol = ref(0)
const dRow = ref(-1)

var interval = 0

onBeforeMount(() =>
{
   InitBoard()
   Board[2][5] = 3
   InitSnake()
   window.addEventListener('keydown', onKeyDown)
})

onMounted(() =>
{
   interval = setInterval(() => RenderSnake(), 250)
})

function onKeyDown(evt)
{
   switch (evt.key)
   {
      case 'ArrowUp':
         if (dRow.value != 1)
         {
            dRow.value = -1
            dCol.value = 0
         }
         break
      case 'ArrowDown':
         if (dRow.value != -1)
         {
            dRow.value = 1
            dCol.value = 0
         }
         break
      case 'ArrowLeft':
         if (dCol.value != 1)
         {
            dRow.value = 0
            dCol.value = -1
         }
         break
      case 'ArrowRight':
         if (dCol.value != -1)
         {
            dRow.value = 0
            dCol.value = 1
         }
         break
   }
}

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
   // let Head = Object.copy(Snake[0])

   let Head = JSON.stringify(Snake[0])
   Head = JSON.parse(Head)

   Head.col = Head.col + dCol.value
   Head.row = Head.row + dRow.value

   if (Head.row < 0 || Head.row > 25 || Head.Col < 0 || Head.Col > 25)
   {
      GameOver()
   }

   Snake[0].seg = 2

   if (Snake.filter((item) => item.row == Head.row && item.col == Head.col).length != 0)
   {
      GameOver()
   }

   // Snake.unshift(Head)

   // Хвост

   if (Board[Head.row][Head.col] == 3)
   {
      let B = NewTarget()
      Board[B.row][B.col] = 3
      Snake.unshift(Head)
      emit('score')
      console.log("Take target")
   } else
   {
      Snake.unshift(Head)
      let Tail = Snake.pop()

      Snake.forEach((cell) => Board[cell.row][cell.col] = cell.seg)
      Board[Tail.row][Tail.col] = 0
   }
}

function NewTarget()
{
   let col = Math.floor(Math.random() * 24)
   let row = Math.floor(Math.random() * 24)

   let a = Snake.filter((item) =>
   {
      if ((item.col == col) && (item.row == row))
      {
         return true
      } else
      {
         return false
      }
   })

   if (a.length != 0)
   {
      return NewTarget()
   } else
   {
      return { col: col, row: row }
   }
}

function GameOver()
{
   clearInterval(interval)
   console.log("Game Over")
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