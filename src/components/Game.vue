<template>
    <div class="flex flex-wrap w-full h-full content-start">
        <Cell
                v-for="(cell, index) in cells"
                :revealed="cell.revealed"
                :is-bomb="cell.isBomb"
                :label="cell.label"
                :marked="cell.marked"
                :id="cell.id"
                :key="index"
                @click.native="revealCell(cell.id)"
                @click.right.native.prevent="toggleMark(cell.id)"
                :style="cellStyle"
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
            gameSize: {
                type: String,
                default: 'S'
            },
            restartRequest: {
                type: Number,
                default: 0
            },
            debugShowBombs: {
                type: Boolean,
                default: false
            }
        },

        watch: {
            restartRequest: function() {
                this.restart()
            },

            debugShowBombs: function (val) {
                this.cells.filter(cell => cell.isBomb).forEach(cell => cell.revealed = val)
            }
        },

        computed: {
            gameWon() {
                return this.cells.every((cell) => {
                    return cell.isBomb || cell.revealed
                })
            },

            cellStyle() {
                return this.boardSizes[this.gameSize].cellStyle
            },

            boardSize() {
                return this.boardSizes[this.gameSize].value
            }
        },

        data() {
            return {
                cells: [],
                won: false,
                boardSizes: {
                    'S': {value: 8, cellStyle: "width: 12.5%; height: 12.5%; font-size: 300%"},
                    'M': {value: 12, cellStyle: "width: 8.3333%; height: 8.3333%; font-size: 200%"},
                    'L': {value: 16, cellStyle: "width: 6.25%; height: 6.25%;"},
                }
            }
        },

        methods: {
            revealCell(index) {
                if (this.cells[index].revealed) return
                console.log("revealing cell with index " + index)

                if (this.cells[index].isBomb) {
                    console.log("Welp. That was a bomb.")
                    return this.loose()
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

                if (this.gameWon) {
                    return this.win()
                }
            },

            toggleMark(index) {
                this.cells[index].marked = ! this.cells[index].marked
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
                this.$nextTick(function() {
                    alert("BOOM! The mine explodes, ripping you apart. You lost. :(")
                })
            },

            win() {
                if (!this.won) {
                    this.won = true
                    this.revealAll()
                    this.$nextTick()
                    alert("Yes! You won!")
                    return true
                }
            },

            restart() {
                let cells = []

                let bS = this.boardSize

                // noinspection JSCheckFunctionSignatures
                for (let i = 0; i < Math.pow(bS, 2); i++) {
                    cells[i] = {
                        id: i,
                        revealed: false,
                        isBomb: Math.floor((Math.random() * 6) + 1) === 1,
                        label: "&nbsp;",
                        marked: false,
                    }
                }

                this.cells = cells
            }
        },

        created() {
            this.restart()
        }
    }
</script>

<style scoped>

</style>