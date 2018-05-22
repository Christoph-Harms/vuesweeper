<template>
    <div id="app" class="mt-8">
        <div class="w-full m-auto md:w-1/2 p-4 mt-6" style="height: 50vw">
            <div class="flex justify-between p-2">
                <button
                        class="bg-blue hover:bg-blue-dark text-white font-bold py-2 px-4 rounded"
                        @click="restart()"
                >
                    New Game
                </button>
                <div class="flex flex-col justify-center items-end">
                    <label for="gamesize" class="mb-1">Select game size:</label>
                    <select v-model="gameSize" id="gamesize">
                        <option disabled value="">Select game size</option>
                        <option v-for="option in gameSizeOptions" :value="option.value" :key="option.value">
                            {{ option.text }}
                        </option>
                    </select>
                </div>
            </div>
            <Dashboard
                    :timer-running="timerRunning"
            ></Dashboard>
            <Game
                    :restart-request="restartCounter"
                    :game-size="gameSize"
                    :debug-show-bombs="debugShowBombs"
                    @gameStarted="startTimer"
                    @gameStopped="stopTimer"
            />
            <input type="checkbox" id="checkbox" v-model="debugShowBombs">
            <label for="checkbox">(Debug) Show bombs</label>
        </div>
    </div>
</template>

<script>
    import './assets/styles/main.css'
    import Game from './components/Game'
    import Dashboard from './components/Dashboard'

    export default {
        name: 'app',
        components: {
            Game,
            Dashboard
        },

        data() {
            return {
                restartCounter: 0,
                gameSize: 'S',
                gameSizeOptions: [
                    {text: 'S (8x8)', value: 'S'},
                    {text: 'M (12x12)', value: 'M'},
                    {text: 'L (16x16)', value: 'L'},
                ],
                debugShowBombs: false,
                timerRunning: false,
            }
        },

        methods: {
            restart() {
                this.restartCounter++
            },

            startTimer() {
                this.timerRunning = true
            },

            stopTimer() {
                this.timerRunning = false
            }
        }
    }
</script>

<style>

</style>
