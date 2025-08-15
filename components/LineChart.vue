<script setup>
import { ref } from 'vue'
import VueApexCharts from 'vue3-apexcharts'

const investmentAmount = ref(10000)
const isNextClicked = ref(false)
const isFirstQuestionActive = ref(true)

const handleNextClick = () => {
    isNextClicked.value = true
    isFirstQuestionActive.value = false
}

const handleBackClick = () => {
    isNextClicked.value = false
    isFirstQuestionActive.value = true
}

const handleFirstQuestionClick = () => {
    isFirstQuestionActive.value = false
}

const series = ref([{
    name: 'Actual Energy Production',
    data: [
        { x: "0hr/day", y: 0 },
        { x: "1hr/day", y: 55000000 },
        { x: "2hr/day", y: 110000000 },
        { x: "3hr/day", y: 165000000 },
        { x: "4hr/day", y: 220000000 },
        { x: "5hr/day", y: 275000000 },
        { x: "6hr/day", y: 330000000 }
    ]
}, {
    name: 'Actual Returns',
    data: [
        { x: "0hr/day", y: 0 },
        { x: "1hr/day", y: 5573500 },
        { x: "2hr/day", y: 13878500 },
        { x: "3hr/day", y: 22183500 },
        { x: "4hr/day", y: 30488500 },
        { x: "5hr/day", y: 38793500 },
        { x: "6hr/day", y: 47098500 }
    ]
}])

const chartOptions = ref({
    chart: {
        type: 'area',
        height: 400,
        toolbar: {
            show: true
        },
        zoom: {
            enabled: true
        }
    },
    colors: ['#417D23', '#3026DB'],
    dataLabels: {
        enabled: false
    },
    stroke: {
        curve: 'straight',
        width: 3
    },
    markers: {
        size: 5,
        colors: ['#417D23', '#3026DB'],
        strokeColors: '#fff',
        strokeWidth: 2,
        hover: {
            size: 7
        }
    },
    fill: {
        type: 'gradient',
        gradient: {
            shadeIntensity: 1,
            opacityFrom: 0.4,
            opacityTo: 1,
            stops: [100, 100, 100]
        }
    },
    xaxis: {
        type: 'numeric',
        min: 2025,
        max: 2055,
        tickAmount: 6,
        labels: {
            formatter: function (val) {
                return val.toString()
            }
        },
    },
    yaxis: {
        min: 0,
        max: 350000000,
        tickAmount: 7,
        labels: {
            formatter: function (val) {
                if (val >= 1000000) {
                    return '$' + (val / 1000000).toFixed(0) + ',000,000'
                }
                return '$' + val.toLocaleString()
            }
        },
    },
    grid: {
        borderColor: '#e5e7eb',
        strokeDashArray: 0,
        xaxis: {
            lines: {
                show: true
            }
        },
        yaxis: {
            lines: {
                show: true
            }
        }
    },
    legend: {
        position: 'top',
        horizontalAlign: 'left',
        markers: {
            width: 40,
            height: 6,
            radius: 0
        }
    },
    tooltip: {
        shared: true,
        intersect: false,
        y: {
            formatter: function (val) {
                return '$' + val.toLocaleString()
            }
        }
    }
})
</script>

<template>
    <div class="bg-grey">
        <div class="flex" style="width: 100%;">
            <div class="">
                <p class="price">${{ investmentAmount.toLocaleString('en-US', {
                    minimumFractionDigits: 2, maximumFractionDigits:
                        2
                }) }}</p>
                <div class="flex">
                    <div class="dots">
                        1
                    </div>
                    <p class="price" :class="{ 'text-black': isFirstQuestionActive }" @click="handleFirstQuestionClick">
                        How much
                        do you want to invest</p>
                </div>
                <div class="flex" v-show="!isNextClicked">
                    <p>investmentAmount</p>
                    <input type="number" v-model="investmentAmount" class="numberInput">
                </div>
                <button class="next-btn" v-show="!isNextClicked" @click="handleNextClick">Next</button>
                <button class="back-btn" v-show="isNextClicked" @click="handleBackClick">Back</button>
                <div class="flex">
                    <div class="dots">
                        2
                    </div>
                    <p class="price" :class="{ 'text-black': isNextClicked }">Do you plan to withdraw your monthly
                        dividends or
                        automatically reinvest them ?
                    </p>
                </div>
            </div>
            <div class="line-chart-container">
                <ClientOnly>
                    <VueApexCharts type="area" height="400" :options="chartOptions" :series="series" />
                </ClientOnly>
            </div>
        </div>
    </div>
</template>

<style scoped>
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
}

.flex {
    display: flex;
    align-items: center;
    gap: 8px;
}

.price {
    font-size: 20px;
    font-weight: 700;
    color: #8A8A8A;
    max-width: 420px;
}

.dots {
    size: 18px;
    min-width: 18px;
    border-radius: 50%;
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: black;
}

.numberInput {
    border: 1px solid #cccccc;
    background-color: transparent;
    padding: 10px 8px;
}

.line-chart-container {
    width: 100%;
    max-width: 475px;
    margin: 0 auto;
    padding: 1rem;
    background: white;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    margin-top: 20px;
}

.loading {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 400px;
    color: #666;
    font-size: 1rem;
}

.next-btn {
    padding: 10px 8px;
    background: greenyellow;
    width: 55px;
    border: none;
    border-radius: 20px;
    cursor: pointer;
}

.back-btn {
    padding: 10px 8px;
    background: goldenrod;
    width: 55px;
    border: none;
    border-radius: 20px;
    cursor: pointer;
}

.text-black {
    color: black !important;
}

.price:hover {
    cursor: pointer;
}

.bg-grey {
    background-color: #F8F8F8;
    border-radius: 20px;
    padding: 20px;
    display: flex;
    justify-content: space-between;
}
</style>