<template>
    <svg viewBox="0 0 50 80">
        <rect height="80" width="50" :fill="backgroundColor"/>
        <path d="M10,10 l5,-5 h20 l5,5 l-5,5 h-20z" :fill="segments.t.active ? color : colorInactive" :stroke="backgroundColor"/>
        <!-- top -->
        <path d="M40,10 l5,5 v25 h-5 l-5,-5 v-20z" :fill="segments.tr.active ? color : colorInactive" :stroke="backgroundColor"/>
        <!-- top right -->
        <path d="M40,40 h5 v25 l-5,5 l-5,-5 v-20z" :fill="segments.br.active ? color : colorInactive" :stroke="backgroundColor"/>
        <!-- bottom right -->
        <path d="M40,70 l-5,5 h-20 l-5,-5 l5,-5 h20z" :fill="segments.b.active ? color : colorInactive" :stroke="backgroundColor"/>
        <!-- bottom -->
        <path d="M10,40 l5,5 v20 l-5,5 l-5,-5 v-25z" :fill="segments.bl.active ? color : colorInactive" :stroke="backgroundColor"/>
        <!-- bottom left -->
        <path d="M10,10 l5,5 v20 l-5,5 h-5 v-25z" :fill="segments.tl.active ? color : colorInactive" :stroke="backgroundColor"/>
        <!-- top left -->
        <path d="M10,40 l5,-5 h20 l5,5 l-5,5 h-20z" :fill="segments.c.active ? color : colorInactive" :stroke="backgroundColor"/>
        <!-- center -->
    </svg>
</template>

<script>
    export default {
        name: "SegmentedDigit",

        props: {
            value: {
                type: Number,
                default: null
            },

            color: {
                type: String,
                default: "#EC3324"
            },

            backgroundColor: {
                type: String,
                default: "#000000"
            }
        },

        computed: {
            colorInactive() {
                return this.lightenDarkenColor(this.color, -175)
            }
        },

        watch: {
            value(newVal) {
                this.displayDigit(newVal)
            }
        },

        data() {
            return {
                segments: {
                    t: {
                        active: false
                    },
                    tr: {
                        active: false
                    },
                    br: {
                        active: false
                    },
                    b: {
                        active: false
                    },
                    bl: {
                        active: false
                    },
                    tl: {
                        active: false
                    },
                    c: {
                        active: false
                    },
                }
            }
        },

        methods: {
            lightenDarkenColor(col) {
                if (col[0] === "#") {
                    col = col.slice(1);
                }

                let num = parseInt(col, 16);

                let r = (num >> 16);

                if (r > 255) r = 255;
                else if (r < 0) r = 0;

                let b = ((num >> 8) & 0x00FF);

                if (b > 255) b = 255;
                else if (b < 0) b = 0;

                let g = (num & 0x0000FF);

                if (g > 255) g = 255;
                else if (g < 0) g = 0;

                return "rgba(" + r + ", " + g + ", " + b + ", 0.2)"
            },

            displayDigit(digit) {
                switch (digit) {
                    case 0:
                        this.segments.t.active = true
                        this.segments.tr.active = true
                        this.segments.br.active = true
                        this.segments.b.active = true
                        this.segments.bl.active = true
                        this.segments.tl.active = true
                        this.segments.c.active = false
                        break;
                    case 1:
                        this.segments.t.active = false
                        this.segments.tr.active = true
                        this.segments.br.active = true
                        this.segments.b.active = false
                        this.segments.bl.active = false
                        this.segments.tl.active = false
                        this.segments.c.active = false
                        break;
                    case 2:
                        this.segments.t.active = true
                        this.segments.tr.active = true
                        this.segments.br.active = false
                        this.segments.b.active = true
                        this.segments.bl.active = true
                        this.segments.tl.active = false
                        this.segments.c.active = true
                        break;
                    case 3:
                        this.segments.t.active = true
                        this.segments.tr.active = true
                        this.segments.br.active = true
                        this.segments.b.active = true
                        this.segments.bl.active = false
                        this.segments.tl.active = false
                        this.segments.c.active = true
                        break;
                    case 4:
                        this.segments.t.active = false
                        this.segments.tr.active = true
                        this.segments.br.active = true
                        this.segments.b.active = false
                        this.segments.bl.active = false
                        this.segments.tl.active = true
                        this.segments.c.active = true
                        break
                    case 5:
                        this.segments.t.active = true
                        this.segments.tr.active = false
                        this.segments.br.active = true
                        this.segments.b.active = true
                        this.segments.bl.active = false
                        this.segments.tl.active = true
                        this.segments.c.active = true
                        break;
                    case 6:
                        this.segments.t.active = true
                        this.segments.tr.active = false
                        this.segments.br.active = true
                        this.segments.b.active = true
                        this.segments.bl.active = true
                        this.segments.tl.active = true
                        this.segments.c.active = true
                        break;
                    case 7:
                        this.segments.t.active = true
                        this.segments.tr.active = true
                        this.segments.br.active = true
                        this.segments.b.active = false
                        this.segments.bl.active = false
                        this.segments.tl.active = false
                        this.segments.c.active = false
                        break;
                    case 8:
                        this.segments.t.active = true
                        this.segments.tr.active = true
                        this.segments.br.active = true
                        this.segments.b.active = true
                        this.segments.bl.active = true
                        this.segments.tl.active = true
                        this.segments.c.active = true
                        break;
                    case 9:
                        this.segments.t.active = true
                        this.segments.tr.active = true
                        this.segments.br.active = true
                        this.segments.b.active = true
                        this.segments.bl.active = false
                        this.segments.tl.active = true
                        this.segments.c.active = true
                        break;
                    default:
                        this.segments.t.active = false
                        this.segments.tr.active = false
                        this.segments.br.active = false
                        this.segments.b.active = false
                        this.segments.bl.active = false
                        this.segments.tl.active = false
                        this.segments.c.active = false
                        break;
                }
            }
        },

        created() {
            this.displayDigit(this.value)
        }
    }
</script>

<style scoped>

</style>