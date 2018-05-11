<template>
    <div class="flex flex-wrap">
        <Cell
                v-for="(cell, index) in cells"
                :revealed="cell.revealed"
                :is-bomb="cell.isBomb"
                :label="cell.label"
                :marked="cell.marked"
                :id="cell.id"
                :key="index"
                @click.native="revealCell(cell.id)"
        />
    </div>
</template>

<script>
    import Cell from './Cell'
    export default {
        name: "Game",
        components: {
            Cell
        },

        props: {
            boardSize: {
                type: Number,
                default: 8
            }
        },

        computed: {
            gameWon() {
                return this.cells.every((cell) => {
                    return cell.isBomb || cell.revealed
                })
            }
        },

        data() {
            return {
                cells: [],
                won: false
            }
        },

        methods: {
            revealCell(index) {
                if (this.cells[index].revealed) return
                console.log("revealing cell with index " + index)

                if (this.cells[index].isBomb) {
                    this.loose()
                    return
                }

                let neighbourIndexes = this.getNeighbourIndexes(index)

                let that = this

                let numBombs = neighbourIndexes.reduce((carry, item) => {
                    if (that.cells[item].isBomb) {
                        carry += 1
                    }
                    return carry
                }, 0)

                console.log("Found " + numBombs + " adjacent bombs.")

                this.cells[index].revealed = true

                if (numBombs === 0) {
                    let vm = this
                    neighbourIndexes.forEach(nIndex => vm.revealCell(nIndex))
                } else {
                    this.cells[index].label = numBombs
                }

                if (this.gameWon) this.win()
            },

            revealAll() {
                this.cells.forEach(cell => cell.revealed = true)
            },

            getNeighbourIndexes(index) {
                let nIndexes = [index - this.boardSize, index + this.boardSize]

                if (index % this.boardSize === 0) {
                    // left border of board
                    nIndexes.push(
                        index + this.boardSize + 1,
                        index - this.boardSize + 1,
                        index + 1
                    )
                } else if ((index + 1) % this.boardSize === 0) {
                    // right border of board
                    nIndexes.push(
                        index + this.boardSize - 1,
                        index - this.boardSize - 1,
                        index - 1
                    )
                } else {
                    // middle of board
                    nIndexes.push(
                        index + this.boardSize + 1,
                        index - this.boardSize + 1,
                        index + 1,
                        index + this.boardSize - 1,
                        index - this.boardSize - 1,
                        index - 1
                    )
                }

                // filter out-of-bounds indices and return
                return nIndexes.filter(nIndex => nIndex >= 0 && nIndex < this.cells.length)
            },

            loose() {
                this.revealAll()
                alert("BOOM! The mine explodes, ripping you apart. You lost. :(")
            },

            win() {
                if (!this.won) {
                    this.won = true
                    this.revealAll()
                    alert("Yes! You won!")
                }
            }
        },

        created() {
            let cells = []

            let bS = this.boardSize

            // noinspection JSCheckFunctionSignatures
            for (let i = 0; i < Math.pow(bS, 2); i++) {
                cells[i] = {
                    id: i,
                    revealed: false,
                    isBomb: Math.floor((Math.random() * 6) + 1) === 1,
                    label: "i" + i,
                    marked: false,
                }
            }

            this.cells = cells
        }
    }
</script>

<style scoped>

</style>