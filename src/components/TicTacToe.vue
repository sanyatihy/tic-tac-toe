<template>
    <div>
        <p>Current player: {{ currentPlayer }}</p>
        <div class="game-board">
            <div v-for="(cell, index) in board" :key="index" class="cell" @click="handleClick(index)">
                {{ cell }}
            </div>
        </div>
        <button @click="resetGame" class="reset-button">Reset Game</button>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
    data() {
        return {
            board: Array(9).fill(''),
            currentPlayer: 'X',
            gameOver: false,
            winningLine: [],
        };
    },

    methods: {
        handleClick(index: number): void {
            if (this.board[index] || this.gameOver) return;

            this.board[index] = this.currentPlayer;

            if (this.checkWin(this.currentPlayer)) {
                this.gameOver = true;
                setTimeout(() => {
                    alert(`${this.currentPlayer} wins!`);
                    this.resetGame();
                }, 200);
            } else if (this.board.every(cell => cell !== '')) {
                this.gameOver = true;
                setTimeout(() => {
                    alert("It's a draw!");
                    this.resetGame();
                }, 200);
            } else {
                this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
            }
        },

        checkWin(player: string): boolean {
            const winPositions = [
                [0, 1, 2], [3, 4, 5], [6, 7, 8],
                [0, 3, 6], [1, 4, 7], [2, 5, 8],
                [0, 4, 8], [2, 4, 6]
            ];
            return winPositions.some(line => line.every(pos => this.board[pos] === player));
        },

        resetGame(): void {
            this.board = Array(9).fill('');
            this.currentPlayer = 'X';
            this.gameOver = false;
        },
    },
});
</script>

<style scoped>
.game-board {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 5px;
    width: 90vw;
    max-width: 300px;
    margin: 0 auto;
    padding: 10px;
}

.cell {
    width: calc(30vw - 10px);
    height: calc(30vw - 10px);
    max-width: 100px;
    max-height: 100px;
    background-color: #f0f0f0;
    border: 1px solid #ccc;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 24px;
    font-weight: bold;
    cursor: pointer;
}

.cell:hover {
    background-color: #e0e0e0;
}

.reset-button {
  display: block;
  margin: 20px auto;
  padding: 10px 20px;
  background-color: #2c3e50;
  color: #fff;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.reset-button:hover {
  background-color: #1a2533;
}
</style>
