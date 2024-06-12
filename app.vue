<script setup>
import { ref, computed, h } from "vue";
import { Icon } from "#components";

const cross = h(Icon, { name: "mdi:close" });
const nought = h(Icon, { name: "mdi:checkbox-blank-circle-outline" });

const currentPlayer = ref("X");
const board = ref(Array(9).fill(""));

const winningCombinations = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  [0, 4, 8],
  [2, 4, 6],
];

const calculateWinner = (board) => {
  for (const [a, b, c] of winningCombinations) {
    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
      return board[a];
    }
  }
  return null;
};

const winner = computed(() => calculateWinner(board.value));
const isBoardFull = computed(() => board.value.every((cell) => cell !== ""));
const tie = computed(() => isBoardFull.value && !winner.value);

const makeMove = (index) => {
  if (winner.value || board.value[index]) return;
  board.value[index] = currentPlayer.value;
  currentPlayer.value = currentPlayer.value === "X" ? "O" : "X";
};

const replay = () => {
  currentPlayer.value = "X";
  board.value = Array(9).fill("");
};
</script>

<template>
  <main class="grid space-y-20">
    <h1 class="text-6xl font-bold">Tic Tac Toe</h1>
    <div>
      <div class="mb-4 flex justify-between">
        <span
          v-if="!winner && !tie"
          class="pointer-default select-none rounded-3xl border-2 border-black bg-teal-500 px-6 font-bold uppercase"
        >
          <strong class="mr-1">{{ currentPlayer }}</strong> turn
        </span>
        <span
          v-if="winner"
          class="rounded-3xl border-2 border-black bg-teal-500 px-6 font-bold uppercase"
        >
          <strong class="mr-1">{{ winner }}</strong> wins!
        </span>
        <span
          v-else-if="tie"
          class="rounded-3xl border-2 border-black bg-teal-500 px-6 font-bold uppercase"
        >
          it's a tie!
        </span>
        <button
          @click="replay"
          class="rounded-3xl border-2 border-black bg-red-500 px-6 font-bold uppercase"
        >
          replay
        </button>
      </div>
      <div class="grid grid-cols-3 gap-4">
        <div
          v-for="(cell, index) in board"
          :key="index"
          @click="makeMove(index)"
          :class="[
            'flex h-24 w-24 cursor-pointer items-center justify-center rounded-2xl border-2 border-b-4 border-black text-6xl hover:border-b-2 hover:bg-[#C8C5B6]',
            { 'text-red-500': cell === 'X', 'text-teal-500': cell === 'O' },
          ]"
        >
          <component
            :is="cell === 'X' ? cross : cell === 'O' ? nought : 'span'"
          />
        </div>
      </div>
    </div>
    <h2 class="text-xl uppercase">Developed by <strong>Igor Fiorio</strong></h2>
  </main>
</template>
