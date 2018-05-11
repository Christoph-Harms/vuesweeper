<template>
    <div
            class="border-grey-lighter border-2 w-10 h-10 text-white text-2xl flex justify-center flex-col flex items-center"
            :class="[revealed ? revealedClass : unrevealedClass]"
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
                type: Number,
                default: null,
            },
            marked: {
                type: Boolean,
                default: true,
            }
        },

        computed: {
            labelHtml() {
                if (this.marked) return "<i class=\"fas fa-exclamation-triangle\"></i>"

                return this.label
            }
        },

        data() {
            return {
                unrevealedClass: "bg-grey-dark",
                revealedClass: "bg-grey-light",
            }
        },
    }
</script>
