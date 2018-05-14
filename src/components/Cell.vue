<template>
    <div
            class="border-red-lighter border-2 overflow-hidden flex items-center justify-center"
            :class="[revealed ? revealedClass : unrevealedClass, textClass]"
    >
        <p v-html="labelHtml" class="block"></p>
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
                default: "&nbsp;",
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

                if (this.isBomb && this.revealed) return "<i class=\"fas fa-bomb\"></i>"

                return this.label
            },

            textClass() {
                switch (this.label) {
                    case 1: return "text-blue-light"
                    case 2: return "text-green"
                    case 3: return "text-red"
                    case 4: return "text-blue-dark"
                    case 5: return "text-brown"
                    case 6: return "text-indigo"
                    case 7: return "text-purple"
                    case 8: return "text-pink"
                }
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
