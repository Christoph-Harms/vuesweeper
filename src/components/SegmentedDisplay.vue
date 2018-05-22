<template>
    <div>
        <segmented-digit
                v-for="digit in digits"
                :value="digit"
                style="height: inherit"
                :background-color="digitBgColor"
                :color="digitColor"
        />
    </div>
</template>

<script>
    import SegmentedDigit from "./includes/SegmentedDigit"

    export default {
        components: {
            SegmentedDigit
        },

        name: "SegmentedDisplay",

        props: {
            value: {
                type: Number,
                default: null
            },
            numDigits: {
                type: Number,
                default: 1
            },
            digitColor: {
                type: String,
            },
            digitBgColor: {
                type: String
            }
        },

        computed: {
            digits() {
                let digits = []
                if (this.value === null) {
                    digits = [null]
                } else {
                    digits = this.value.toString().split('').map(Number)
                }

                while (digits.length !== this.numDigits) {
                    if (digits.length < this.numDigits) {
                        digits.unshift(null)
                    } else {
                        digits.shift()
                    }
                }

                return digits
            }
        },

        data() {
            return {}
        }
    }


</script>

<style scoped>

</style>