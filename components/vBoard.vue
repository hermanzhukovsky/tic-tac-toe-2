<template>
	<div class="body-game__board board">
		<div class="board__body">
			<button 
				class="board__square"
				@click="onSquareClick(index)" 
				:key="index" 
				:class="{ _highlighted: square.isHighlighted }"
				v-for="(square, index) in squares" type="button"
				>
				{{ square.value }}
			</button>
		</div>
	</div>
</template>


<script>
// import vSquare from '@/components/vSquare';

export default {
	
	name: "vBoard",
	components: {
		// vSquare
	},
	data() {
		return {
			firstPlayerTurn: true,
			secondPlayerTurn: false,
			isGameOver: false,
			squares: [
				{ id: 1, value: "", isHighlighted: false }, 
				{ id: 2, value: "", isHighlighted: false },
				{ id: 3, value: "", isHighlighted: false },
				{ id: 4, value: "", isHighlighted: false },
				{ id: 5, value: "", isHighlighted: false },
				{ id: 6, value: "", isHighlighted: false },
				{ id: 7, value: "", isHighlighted: false },
				{ id: 8, value: "", isHighlighted: false },
				{ id: 9, value: "", isHighlighted: false },
			],
			lines: [
				[0, 4, 8],
				[2, 4, 6],
				[0, 1, 2],
				[3, 4, 5],
				[6, 7, 8],
				[0, 3, 6],
				[1, 4, 7],
				[2, 5, 8]
			]
		}
	},
	methods: {
		onSquareClick(index) {
			const square = this.squares[index];
			if (this.firstPlayerTurn && square.value === "") {
				square.value = 'X';
				localStorage.setItem(index, 'X');
				this.firstPlayerTurn = false;
				this.secondPlayerTurn = true;
			} else if (this.secondPlayerTurn && square.value === "") {
				square.value = 'O';
				localStorage.setItem(index, 'O');
				this.firstPlayerTurn = true;
				this.secondPlayerTurn = false;
			}
			this.calculateWinner();
		},
		calculateWinner() {
			this.lines.forEach(line => {
				const [firstIndex, secondIndex, thirdIndex] = line;
				if (
					this.squares[firstIndex].value === "X" && 
					this.squares[secondIndex].value === "X" && 
					this.squares[thirdIndex].value === "X"
				) {
						this.squares[firstIndex].isHighlighted = true;
						this.squares[secondIndex].isHighlighted = true;
						this.squares[thirdIndex].isHighlighted = true;
					}
				if (
					this.squares[firstIndex].value === "O" && 
					this.squares[secondIndex].value === "O" && 
					this.squares[thirdIndex].value === "O"
				) {
						this.squares[firstIndex].isHighlighted = true;
						this.squares[secondIndex].isHighlighted = true;
						this.squares[thirdIndex].isHighlighted = true;
					}
			});
		}
	},
}
</script>

<style lang="scss">
.board {
	display: flex;
	align-items: center;
	&__body {
		display: flex;
		flex-wrap: wrap;
		width: 240px;
	}
	&__square {
		font-size: 40px;
		height: 80px;
		flex: 0 0 80px;
		line-height: 80px;
		border: 1px solid #fff;
		margin-top: -1px;
		margin-right: -1px;
		color: inherit;
		&._highlighted {
			color: rgb(113, 241, 63);
		}
	}
}
</style>