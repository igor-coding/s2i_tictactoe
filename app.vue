<script setup>
import { ref, computed } from 'vue'
import { Icon } from '#components'

const cross = h(Icon, { name: 'mdi:close' });
const nought = h(Icon, { name: 'mdi:checkbox-blank-circle-outline' });

const currentPlayer = ref('X')
const board = ref([
  [''], [''], [''],
  [''], [''], [''],
  [''], [''], ['']
])

const calculateWinner = (board) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ]

  for (const [a, b, c] of lines) {
    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
      return board[a]
    }
  }
  return null
}

const winner = computed(() => calculateWinner(board.value.flat()))
const isBoardFull = computed(() => board.value.flat().every(cell => cell !== ''))
const tie = computed(() => isBoardFull.value && !winner.value)

const makeMove = (x, y) => {
  if (winner.value || board.value[x][y]) return
  board.value[x][y] = currentPlayer.value
  currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X'
}

const replay = () => {
  board.value = [
    [''], [''], [''],
    [''], [''], [''],
    [''], [''], ['']
  ]
  currentPlayer.value = 'X'
}
</script>

<template>
  <main
    class="overflow-hidden h-screen w-screen p-6 md:space-y-20 flex flex-col items-center justify-around md:justify-center text-center">
    <div class="space-y-2">
      <h1 class="text-6xl font-bold">Tic Tac Toe</h1>
    </div>
    <div class="flex flex-col gap-4">
      <div class="flex justify-between">
        <p class="border-2 border-black rounded-3xl px-6 bg-teal-500 font-bold uppercase pointer-default select-none"
          v-if="!winner && !tie">
          {{ currentPlayer }} turn
        </p>
        <p v-if="winner" class="border-2 border-black rounded-3xl px-6 bg-teal-500 font-bold uppercase">
          {{ winner }} wins!
        </p>
        <p v-else-if="tie" class="border-2 border-black rounded-3xl px-6 bg-teal-500 font-bold uppercase">
          It's a tie!
        </p>
        <button @click="replay"
          class="border-2 border-black rounded-3xl px-6 bg-red-500 font-bold uppercase">replay</button>
      </div>
      <div class="grid grid-cols-3 gap-4">
        <div v-for="(row, rowIndex) in board" :key="rowIndex" class="flex">
          <div v-for="(cell, colIndex) in row" :key="colIndex" @click="makeMove(rowIndex, colIndex)" :class="[
            'h-24 w-24 border-2 border-b-4 hover:border-b-2 hover:bg-[#C8C5B6] border-black rounded-2xl flex items-center justify-center cursor-pointer text-6xl',
            { 'text-red-500': cell === 'X', 'text-teal-500': cell === 'O' }
          ]">
            <component :is="cell === 'X' ? cross : (cell === 'O' ? nought : 'span')" />
          </div>
        </div>
      </div>
    </div>
    <h2 class="text-xl uppercase">Developed by <strong>Igor Fiorio</strong></h2>
  </main>
</template>
