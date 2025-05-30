<template>
  <div class="ttt-wrapper">
    <div class="ttt-container">
      <div class="ttt-status" :style="{ color: accentColor }">
        <template v-if="winner">
          <span v-if="winner === 'draw'">It's a draw!</span>
          <span v-else>Player {{ winner === "X" ? "1" : "2" }} ({{ winner }}) wins!</span>
        </template>
        <template v-else>
          <span>Current: Player {{ currentPlayer === "X" ? "1" : "2" }} ({{ currentPlayer }})</span>
        </template>
      </div>
      <div class="ttt-grid">
        <button
          v-for="(cell, idx) in board"
          :key="idx"
          class="ttt-cell"
          :disabled="!!cell || winner"
          @click="makeMove(idx)"
        >
          {{ cell }}
        </button>
      </div>
      <button class="ttt-reset" @click="resetGame">
        Reset
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// PUBLIC_INTERFACE
// Main container reactive logic for TicTacToe
const board = ref(Array(9).fill(""))
const currentPlayer = ref("X")
const winner = ref(null) // X, O, or 'draw'

// Game colors (light theme + accent)
const primaryColor = "#ffffff"
const secondaryColor = "#000000"
const accentColor = "#2196f3"

// PUBLIC_INTERFACE
// Handles a user move action
function makeMove(idx) {
  if (board.value[idx] || winner.value) return
  board.value[idx] = currentPlayer.value
  if (checkWin(currentPlayer.value)) {
    winner.value = currentPlayer.value
  } else if (board.value.every(cell => cell)) {
    winner.value = "draw"
  } else {
    currentPlayer.value = currentPlayer.value === "X" ? "O" : "X"
  }
}

// PUBLIC_INTERFACE
// Checks if current player has won
function checkWin(player) {
  const winPositions = [
    [0,1,2],[3,4,5],[6,7,8], // rows
    [0,3,6],[1,4,7],[2,5,8], // cols
    [0,4,8],[2,4,6]          // diagonals
  ]
  return winPositions.some(line =>
    line.every(idx => board.value[idx] === player)
  )
}

// PUBLIC_INTERFACE
// Reset the game to initial state
function resetGame() {
  board.value = Array(9).fill("")
  winner.value = null
  currentPlayer.value = "X"
}
</script>

<style scoped>
.ttt-wrapper {
  min-height: 100vh;
  background: #ffffff;
  display: flex;
  align-items: center;
  justify-content: center;
}
.ttt-container {
  background: #fff;
  padding: 2.5rem 2rem 2rem 2rem;
  border-radius: 16px;
  box-shadow: 0 3px 16px rgba(33, 150, 243, 0.07), 0 1.5px 6px rgba(0,0,0,0.07);
  display: flex;
  flex-direction: column;
  align-items: center;
  min-width: 320px;
}
.ttt-status {
  font-size: 1.23rem;
  font-weight: 700;
  margin-bottom: 1.2rem;
  text-align: center;
  color: #2196f3;
  min-height: 2.1rem;
}
.ttt-grid {
  display: grid;
  grid-template-columns: repeat(3, 60px);
  grid-template-rows: repeat(3, 60px);
  gap: 10px;
  margin-bottom: 1.2rem;
}
.ttt-cell {
  background: #fff;
  color: #000;
  font-size: 2.5rem;
  font-weight: bold;
  border: 2px solid #2196f3;
  border-radius: 8px;
  width: 60px;
  height: 60px;
  transition: border 0.15s, background 0.15s;
  cursor: pointer;
  outline: none;
}
.ttt-cell:disabled {
  background: #f4f4f4;
  color: #bdbdbd;
  border-color: #e0e0e0;
  cursor: not-allowed;
}
.ttt-reset {
  margin-top: 0.6rem;
  padding: 0.55rem 2.2rem;
  font-size: 1rem;
  font-weight: 600;
  color: #fff;
  background: #2196f3;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  box-shadow: 0 1.5px 6px rgba(33, 150, 243, 0.08);
  transition: background 0.2s;
}
.ttt-reset:hover {
  background: #1976d2;
}
@media (max-width: 400px) {
  .ttt-container {
    min-width: 95vw;
    padding: 1.6rem 0.2rem;
  }
  .ttt-grid {
    grid-template-columns: repeat(3, 48px);
    grid-template-rows: repeat(3, 48px);
    gap: 7px;
  }
  .ttt-cell {
    width: 48px;
    height: 48px;
    font-size: 1.7rem;
  }
}
</style>
