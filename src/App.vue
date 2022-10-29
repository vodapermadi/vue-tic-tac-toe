<script setup>
import { ref, computed } from 'vue';

const player = ref('X')
const board = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', ''],
])

const calculateWinner = (squares) => {
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
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) { return squares[a] }
  }
  return null;
}

const winner = computed(() => calculateWinner(board.value.flat()))
const makeMove = (x, y) => {
  if (winner.value) return
  if (board.value[x][y] !== '') return

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
  <main class="py-8 text-center dark:bg-indigo-900 dark:text-white min-h-screen">
    <h1 class="font-bold text-3xl capitalize mb-8">tic tac toe</h1>
    <h3 class="font-semibold text-xl mb-8">Player {{ player }}'s turn</h3>
    <div v-for="(row, x) in board" :key="x" class="flex justify-center items-center">
      <div v-for="(cell, y) in row" :key="y" @click="makeMove(x, y)"
        :class="`w-20 h-20 border-2 border-white hover:bg-indigo-700 cursor-pointer text-4xl flex items-center justify-center material-icons-outlined ${cell === 'X' ? 'text-rose-500' : 'text-yellow-500'}`">
        {{ cell === 'X' ? 'close' : cell === 'O' ? 'circle' : '' }}
      </div>
    </div>
    <div v-if="winner" class="mt-4 font-semibold text-xl">Player '{{ player === 'X' ? 'O' : 'X' }}' wins</div>
    <button @click="resetGame"
      class="py-3 px-8 mt-4 rounded font-bold bg-rose-500 hover:bg-pink-600 duration-300 uppercase text-white">reset game</button>
  </main>
</template>
