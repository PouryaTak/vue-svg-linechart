<template>
    <div class="chart bg-dark02 rounded-3xl p-5">
        <div class="flex justify-between items-center w-full mb-3" v-if="title != ''">
            <h6 class="text-light02">
                {{  title  }}
            </h6>
        </div>
        <div class="mb-3 w-full bg-dark01 rounded-xl p-5 relative" style="transform: rotateX(180deg);">
            <svg v-if="Object.keys(charts).length" id="ChartView" width="100%" :height="heigth" class="chart">
                <defs>
                    <linearGradient id="myGradient" gradientTransform="rotate(90)">
                        <stop offset="0%" stop-color="#FFB80000" />
                        <stop offset="100%" stop-color="#FFB80022" />
                    </linearGradient>
                </defs>
                <g class="!translate-y-5">
                    <line x1="0" y1="12" :x2="containerWidth" y2="12" stroke="#474747" />
                    <g v-for="i in 11" :key="i + 'ss'">
                        <text v-if="i === 11" x="0" :y="(i - 1) * ((heigth - 56) / 10) + 12" class="fill-dark10 text-xs"
                            style="transform: rotateX(180deg) translateY(-90%);">{{  0  }}</text>
                        <text v-if="i === 6" x="0" :y="(i - 1) * ((heigth - 56) / 10) + 12" class="fill-dark10 text-xs"
                            style="transform: rotateX(180deg) translateY(-90%);">{{  maxValue / 2  }}</text>
                        <text v-if="i === 1" x="0" :y="(i - 1) * ((heigth - 56) / 10) + 12" class="fill-dark10 text-xs"
                            style="transform: rotateX(180deg) translateY(-90%);">{{  maxValue  }}</text>
                        <line x1="0" :y1="(i - 1) * ((heigth - 56) / 10) + 12" :x2="containerWidth"
                            :y2="(i - 1) * ((heigth - 56) / 10) + 12" stroke="#323232" stroke-dasharray="3 3"
                            stroke-width="1px" />
                    </g>

                    <g class="translate-y-3 translate-x-[40px]">
                        <path fill="none" stroke="#FFB80085" stroke-width="2px" :d="path" class="path" />
                        <path fill="url(#myGradient)" stroke="none" stroke-width="2px" :d="fillPath"
                            class="fadeIn" />
                        <g class="hoverable_items" v-for="(i, index) in points" :key="i.x + 'line' + i.y"
                            @click="selectItem(index)" :class="selectedItem === index ? 'selected' : ''">
                            <text v-if="selectedItem === index && index !== points.length - 1" :x="i.x + 12"
                                :y="(heigth - 56) / 2" class="fill-light02 text-sm"
                                style="transform: rotateX(180deg) translateY(-100%);">{{  i.value + ' ' + unit  }}</text>
                            <text v-if="selectedItem === index && index === points.length - 1" :x="i.x - 80"
                                :y="(heigth - 56) / 2" class="fill-light02 text-sm"
                                style="transform: rotateX(180deg) translateY(-100%);">{{  i.value + ' ' + unit  }}</text>
                            <line :x1="i.x" y1="0" :x2="i.x" :y2="i.y" stroke="#FFB800" class="opacity-0"
                                :style="{ animation: 'pop 150ms linear forwards ' + index * 110 + 'ms' }" />
                            <line :x1="i.x" y1="0" :x2="i.x" y2="400" stroke="#888" class="data_line" />
                            <circle :cx="i.x" :cy="i.y" r="18" class=" fill-[#ffb80044]"
                                v-show="selectedItem === index" />
                            <circle :cx="i.x" :cy="i.y" r="8" class=" cursor-pointer transition-all dot_hover opacity-0"
                                :class="i.y == 0 ? 'fill-accent20' : 'fill-accent00'"
                                :style="{ animation: 'pop 150ms linear forwards ' + index * 100 + 'ms' }" />
                            <text :x="i.x" y="0" class="fill-dark12 text-xs"
                                style="transform: rotateX(180deg) translateY(28px);">{{  i.key  }}</text>
                        </g>
                    </g>
                </g>
            </svg>
            <div v-else>
                <svg id="ChartView" width="100%" :height="heigth" class="chart">

                    <g class="!translate-y-5">
                        <line x1="0" y1="12" :x2="containerWidth" y2="12" stroke="#474747" />
                        <g v-for="i in 11" :key="i + 'ss'">
                            <line x1="0" :y1="(i - 1) * ((heigth - 56) / 10) + 12" :x2="containerWidth"
                                :y2="(i - 1) * ((heigth - 56) / 10) + 12" stroke="#323232" stroke-dasharray="3 3"
                                stroke-width="1px" />
                        </g>
                    </g>
                </svg>
                <svg v-if="loading" width="24" height="24" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"
                    class="absolute top-4 left-5">
                    <path d="M12,1A11,11,0,1,0,23,12,11,11,0,0,0,12,1Zm0,20a9,9,0,1,1,9-9A9,9,0,0,1,12,21Z"
                        transform="translate(12, 12) scale(0)" fill="#FFB800">
                        <animateTransform id="a" begin="0;b.begin+0.6s" attributeName="transform" calcMode="spline"
                            type="translate" dur="1.2s" values="12 12;0 0" keySplines=".52,.6,.25,.99" />
                        <animateTransform begin="0;b.begin+0.6s" attributeName="transform" calcMode="spline"
                            additive="sum" type="scale" dur="1.2s" values="0;1" keySplines=".52,.6,.25,.99" />
                        <animate begin="0;b.begin+0.6s" attributeName="opacity" calcMode="spline" dur="1.2s"
                            values="1;0" keySplines=".52,.6,.25,.99" />
                    </path>
                    <path d="M12,1A11,11,0,1,0,23,12,11,11,0,0,0,12,1Zm0,20a9,9,0,1,1,9-9A9,9,0,0,1,12,21Z"
                        transform="translate(12, 12) scale(0)" fill="#FFB800">
                        <animateTransform id="b" begin="a.begin+0.6s" attributeName="transform" calcMode="spline"
                            type="translate" dur="1.2s" values="12 12;0 0" keySplines=".52,.6,.25,.99" />
                        <animateTransform begin="a.begin+0.6s" attributeName="transform" calcMode="spline"
                            additive="sum" type="scale" dur="1.2s" values="0;1" keySplines=".52,.6,.25,.99" />
                        <animate begin="a.begin+0.6s" attributeName="opacity" calcMode="spline" dur="1.2s" values="1;0"
                            keySplines=".52,.6,.25,.99" />
                    </path>
                </svg>
            </div>

        </div>
        <div>
            <slot />
        </div>
    </div>
</template>
<script>

export default {
    name: "LineChart",
    data() {
        return {
            containerWidth: 0,
            space: 30,
            heigth: 250,
            points: [],
            path: "",
            fillPath: "",
            fillPathPostNode: "",
            selectedItem: null,
            maxValue: "",
            loading: false,
        };
    },
    props: {
        title: {
            type: String,
            default: ''
        },
        charts: {
            type: Object,
            default: () => { }
        },
        unit: {
            type: String,
            default: ''
        },
    },
    methods: {
        selectItem(index) {
            this.selectedItem = index;
        },
        getclientWidth() {
            this.containerWidth = document.querySelector("#ChartView").clientWidth;
            this.space = Math.floor((this.containerWidth - 50) / (Object.keys(this.charts).length - 0.75));
        },
        normalizer(val, min = 0, max = 10) {
            return (val - min) / (max - min);
        },
        convertDataToPoints() {
            let startX = 0;
            Array.prototype.max = function () {
                return Math.max.apply(null, this);
            };
            this.maxValue = Object.values(this.charts).max();
            const increaserValue = (10 / this.maxValue) * 10;
            for (let key in this.charts) {
                this.points.push({ x: this.space * startX, y: (this.normalizer(this.charts[key]) * increaserValue) * ((this.heigth - 56) / 10), key: key, value: this.charts[key] });
                startX += 1;
            }
            this.fillPathPostNode = `L${this.space * (startX - 1)},${0}`;
        },
        catmullRom2bezier(points) {
            let result = [];
            for (let i = 0; i < points.length - 1; i++) {
                let p = [];
                p.push({
                    x: points[Math.max(i - 1, 0)].x,
                    y: points[Math.max(i - 1, 0)].y
                });
                p.push({
                    x: points[i].x,
                    y: points[i].y
                });
                p.push({
                    x: points[i + 1].x,
                    y: points[i + 1].y
                });
                p.push({
                    x: points[Math.min(i + 2, points.length - 1)].x,
                    y: points[Math.min(i + 2, points.length - 1)].y
                });
                let bp = [];
                bp.push({
                    x: ((-p[0].x + 6 * p[1].x + p[2].x) / 6),
                    y: ((-p[0].y + 6 * p[1].y + p[2].y) / 6)
                });
                bp.push({
                    x: ((p[1].x + 6 * p[2].x - p[3].x) / 6),
                    y: ((p[1].y + 6 * p[2].y - p[3].y) / 6)
                });
                bp.push({
                    x: p[2].x,
                    y: p[2].y
                });
                // result.push(bp);
                const pos = bp.map(i => {
                    return {
                        x: i.x < 0 ? 0 : i.x,
                        y: i.y < 0 ? 0 : i.y
                    }
                })
                result.push(pos)
            }
            return result;
        },
        createPath(points) {
            let result = "M" + points[0].x + "," + points[0].y + " ";
            let catmull = this.catmullRom2bezier(points);
            for (let i = 0; i < catmull.length; i++) {
                result += "C" + catmull[i][0].x + "," + catmull[i][0].y + " " + catmull[i][1].x + "," + catmull[i][1].y + " " + catmull[i][2].x + "," + catmull[i][2].y + " ";
            }
            this.path = result;
        },
        createFillPath() {
            const path = this.path.substring(1)
            this.fillPath = `M${0},${0} ` + path + this.fillPathPostNode
        },
        initializer() {
            this.getclientWidth();
            this.convertDataToPoints();
            this.createPath(this.points);
            this.createFillPath()
            this.selectedItem = this.points.length - 1;
        }
    },
    mounted() {
        this.loading = true
        this.getclientWidth();
        if (Object.keys(this.charts).length) {
            this.initializer()
        }
    },
    watch: {
        charts: {
            handler(val) {
                if (Object.keys(this.charts).length) {
                    this.loading = false
                    this.initializer()
                }
            },
            deep: true

        }
    },
    setup() {

    }
}
</script>
<style lang="postcss">

.hoverable_items {
    &.selected {
        .dot_hover {
            /* filter: drop-shadow(0 0 20px #FFB800); */
            fill: #fff;
        }

        .data_line {
            opacity: 1;
        }
    }

    .data_line {
        opacity: 0;
    }

    &:hover {
        .dot_hover:hover {
            /* filter: drop-shadow(0 0 20px #FFB800); */
            fill: #fff
        }
    }
}

.path {
    stroke-dasharray: 2000;
    stroke-dashoffset: 2000;
    animation: dash 2s ease-in-out forwards;
}

@keyframes dash {
    to {
        stroke-dashoffset: 0;
    }
}

.fadeIn {
    animation: fadeIn 1s linear forwards;
}

@keyframes fadeIn {
    from {
        opacity: 0;
        transform: rotateX(95deg);
    }

    to {
        opacity: 1;
        transform: rotateX(0deg);
    }
}

@keyframes pop {
    to {
        opacity: 1;
    }
}
</style>