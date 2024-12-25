<script setup>
import { ref, computed } from 'vue';

const player = ref('X');
const board = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', ''],
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
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
      return board[a];
    }
  }
  return null;
}

const winner = computed(() => calculateWinner(board.value.flat()));
const makeMove = (x, y) => {
  if (winner.value) return
  if (board.value[x][y]) return

  board.value[x][y] = player.value
  player.value = player.value === 'X' ? 'O' : 'X'
}

const resetGame = () => {
  board.value = [
    ['', '', ''],
    ['', '', ''],
    ['', '', ''],
  ]
  player.value = 'X'
}
</script>

<template>
  <main class="pt-8 text-center">
    <h1 class="mb-8 text-5xl text-teal-300 font-bold uppercase"> Tic Tac Toe</h1>
    <h3 class="text-xl mb-4"> Player {{ player }}'s turn</h3>

    <div class="flex flex-col items-center mb-8">
      <div v-for="(row, x) in board" :key="x" class="flex">
        <div v-for="(cell, y) in row" :key="y" @click="makeMove(x, y)"
          :class="`border border-white w-24 h-24 hover:bg-zinc-700 flex items-center justify-center material-icons-outlined text-4xl cursor-pointer ${cell === 'X' ? 'text-teal-500' : 'text-cyan-400'}`">
          {{ cell === 'X' ? 'close' : cell === 'O' ? 'circle' : '' }}
        </div>
      </div>
    </div>
    <div class="text-center">
      <h2 v-if="winner" class="text-4xl font-bold mb-8">Player {{ winner }} wins!</h2>
      <button @click="resetGame"
        class="px-4 py-2 bg-teal-500 rounded uppercase font-bold hover:bg-teal-600 duration-300">Reset</button>
    </div>

    <footer class="fixed bottom-0 left-0 w-full text-center p-2">
      <p><strong>thayllaa</strong> &copy; 2024</p>
    </footer>
  </main>
</template>

<style>
</style>