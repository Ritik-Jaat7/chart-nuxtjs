<script setup>
import { ref, watch } from 'vue'
import VueApexCharts from 'vue3-apexcharts'

const investmentAmount = ref(250000)
const isNextClicked = ref(false)
const isFirstQuestionActive = ref(true)
const isMonthlySelected = ref(true)
const hoursPerDay = ref(4.5);
const reinvest = ref(false);


const handleNextClick = () => {
    isNextClicked.value = true
    isFirstQuestionActive.value = false
    console.log('Next button clicked')
    logCurrentValues()
}

const handleBackClick = () => {
    isNextClicked.value = false
    isFirstQuestionActive.value = true
    console.log('Back button clicked')
    logCurrentValues()
}

const handleFirstQuestionClick = () => {
    isFirstQuestionActive.value = false
}

const toggleSelection = () => {
    isMonthlySelected.value = !isMonthlySelected.value
    console.log('Toggle Selection:', isMonthlySelected.value ? 'Monthly' : 'One-time')
    logCurrentValues()
}

const logCurrentValues = () => {
    console.log('=== Current Values ===')
    console.log('Investment Amount:', investmentAmount.value)
    console.log('Frequency:', isMonthlySelected.value ? 'Monthly' : 'One-time')
    console.log('Hours Per Day:', hoursPerDay.value)
    console.log('Reinvest:', reinvest.value)
    console.log('=====================')
}

// Watch for changes in investment amount
watch(investmentAmount, (newValue, oldValue) => {
    console.log('Investment Amount changed from', oldValue, 'to', newValue)
    logCurrentValues()
})

// Watch for toggle changes
watch(isMonthlySelected, (newValue) => {
    console.log('Toggle changed to:', newValue ? 'Monthly' : 'One-time')
})

// Watch for hours per day changes
watch(hoursPerDay, (newValue, oldValue) => {
    console.log('Hours Per Day changed from', oldValue, 'to', newValue)
    logCurrentValues()
})

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
            show: false
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
        <h5 class="heading">Returns Calculator</h5>
        <div class="flex max-w-1010 " style="justify-content: space-between;">
            <div class="">
                <p class="price" style="color: black;">${{ investmentAmount.toLocaleString('en-US', {
                    minimumFractionDigits: 2, maximumFractionDigits:
                        2
                }) }}</p>
                <p>Principal Invested</p>
            </div>
            <div class="">
                <p class="price" style="color: #6C91BE;">$1380.69</p>
                <p>Est. Dividends Recevied</p>
            </div>
        </div>
        <div class="flex flex-col max-w-1100" style="width: 100%;">
            <div class="">
                <div class="flex">
                    <div class="dots"
                        :class="{ 'dots-active': isFirstQuestionActive, 'dots-inactive': !isFirstQuestionActive }">
                        1
                    </div>
                    <p class="price" :class="{ 'text-black': isFirstQuestionActive }" @click="handleFirstQuestionClick">
                        How much
                        do you want to invest</p>
                </div>
                <div class="flex">
                    <div style="width: 2px; background-color: #8A8A8A; height: 100px; margin-left: 8px;"
                        v-show="!isNextClicked"></div>
                    <div class="">
                        <div style="margin-left: 35px;" class="flex flex-col items-sm-start" v-show="!isNextClicked">
                            <p>investmentAmount</p>
                            <input type="number" v-model="investmentAmount" class="numberInput">
                        </div>

                        <div style="margin: 20px 0 20px 35px;" v-show="!isNextClicked">
                            <div class="toggle-container">
                                <span class="toggle-label" :class="{ 'active': !isMonthlySelected }">One-time</span>
                                <div class="toggle-switch" @click="toggleSelection">
                                    <div class="toggle-slider" :class="{ 'active': isMonthlySelected }"></div>
                                </div>
                                <span class="toggle-label" :class="{ 'active': isMonthlySelected }">Monthly</span>
                            </div>
                        </div>
                        <div style="margin-left: 35px; margin-top: 15px;" class="flex">
                            <p class="back-btn" :class="{ 'text-black': isNextClicked }" @click="handleBackClick">Back
                            </p>
                            <button class="next-btn" v-show="!isNextClicked" @click="handleNextClick">Next ></button>
                        </div>
                    </div>
                </div>
                <div class="flex" style="align-items: baseline !important;">
                    <div class="dots" :class="{ 'dots-active': isNextClicked, 'dots-inactive': !isNextClicked }">
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
@import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');

* {
    font-family: "Poppins", sans-serif;
}

.max-w-1100 {
    max-width: 1000px;
    margin: 0 auto;
}

.justify-between {
    justify-content: space-between;
}

.heading {
    text-align: center;
    font-weight: 300;
    font-size: 14px;
    color: black;
    margin: 0 auto 32px auto;
}

.max-w-1010 {
    max-width: 1010px;
    margin: 0 auto;
}


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
    size: 24px;
    min-width: 24px;
    max-height: 24px;
    border-radius: 50%;
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #24334F;
}

.dots-active {
    background-color: black !important;
}

.dots-inactive {
    background-color: #9199A7 !important;
}

.numberInput {
    border: 1px solid #cccccc;
    background-color: transparent;
    padding: 10px 8px;
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
    padding: 10px 18px;
    background: #87B546;
    border: none;
    border-radius: 20px;
    cursor: pointer;
    color: white;
}

.back-btn {
    color: #C0C0C0;
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
    max-width: 1920px;
    margin: 0 auto;
}

.toggle-container {
    display: flex;
    align-items: center;
    gap: 15px;
}

.toggle-label {
    font-size: 16px;
    font-weight: 500;
    color: #8A8A8A;
    transition: color 0.3s ease;
}

.toggle-label.active {
    color: black;
}

.toggle-switch {
    position: relative;
    width: 60px;
    height: 30px;
    background-color: #7BB3FF;
    border-radius: 15px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.toggle-slider {
    position: absolute;
    top: 3px;
    left: 3px;
    width: 24px;
    height: 24px;
    background-color: white;
    border-radius: 50%;
    transition: transform 0.3s ease;
}

.toggle-slider.active {
    transform: translateX(30px);
}

@media (max-width:768px) {
    .flex-col {
        flex-direction: column;
    }

    .m-0 {
        margin: 0;
    }

    .items-sm-start {
        align-items: start !important;
    }
}

@media (min-width:768px) {
    .line-chart-container {
        width: 100%;
        max-width: 475px;
        margin: 0 0 0 auto;
        padding: 1rem;
        border-radius: 8px;
        margin-top: 20px;
    }

}
</style>