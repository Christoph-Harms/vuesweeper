<template>
    <div class="flex justify-between items-center bg-black">
        <div class="flex justify-start items-center">
            <segmented-display
                    :num-digits="2"
                    :value="numMarkers"
                    digit-color="#f9acaa"
                    class="h-12"
            ></segmented-display>
            <span class="text-5xl text-red-lightest mx-2">/</span>
            <segmented-display
                    :num-digits="2"
                    :value="numBombs"
                    digit-color="#f9acaa"
                    class="h-12"
            ></segmented-display>
        </div>
        <segmented-display
                :num-digits="5"
                :value="value"
                digit-color="#f9acaa"
                class="h-12"
        ></segmented-display>
    </div>
</template>

<script>
    import SegmentedDisplay from './SegmentedDisplay'

    export default {
        name: "Dashboard",

        props: {
            timerRunning: {
                type: Boolean,
                default: false,
            }
        },

        watch: {
            timerRunning(newVal) {
                if (newVal) {
                    this.startTimer()
                } else {
                    this.stopTimer()
                }
            }
        },

        components: {
            SegmentedDisplay
        },

        data() {
            return {
                interval: {},
                value: 0,
                numBombs: 12,
                numMarkers: 4
            }
        },

        methods: {
            startTimer() {
                console.log("starting timer")
                this.resetTimer()
                this.interval = window.setInterval(() => this.value++, 1000)
            },

            stopTimer() {
                console.log("stopping timer")
                window.clearInterval(this.interval)
            },

            resetTimer() {
                this.value = 0
            }
        }
    }
</script>

<style scoped>

</style>