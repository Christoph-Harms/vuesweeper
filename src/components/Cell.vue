<template>
    <div
            class="border-red-lighter border-2 w-1/6 h-auto text-white text-5xl flex justify-center flex-col items-center"
            :class="[revealed ? revealedClass : unrevealedClass]"
            :style="dimensionStyles"
    >
        <p v-html="labelHtml"></p>
    </div>
</template>

<script>
    export default {
        name: 'Cell',

        props: {
            revealed: {
                type: Boolean,
                default: false,
            },
            isBomb: {
                type: Boolean,
                default: false,
            },
            label: {
                type: [String, Number],
                default: null,
            },
            marked: {
                type: Boolean,
                default: true,
            },
            id: {
                type: Number
            }
        },

        computed: {
            labelHtml() {
                if (this.marked) return "<i class=\"fas fa-exclamation-triangle\"></i>"

                if (this.isBomb) return "<i class=\"fas fa-bomb\"></i>"

                return this.label
            }
        },

        data() {
            return {
                unrevealedClass: "bg-grey-dark",
                revealedClass: "bg-grey-light",

                dimensionStyles: {
                    width: '12.5vw',
                    height: '12.5vh'
                },
            }
        },
    }
</script>
