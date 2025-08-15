<script setup lang="ts">
import { ref } from 'vue'
import VueApexCharts from 'vue3-apexcharts'

const series = ref([{
    name: 'PRODUCT A',
    data: [1, 5]
}, {
    name: 'PRODUCT B',
    data: [10, 13],
},])

const chartOptions = ref({
    chart: {
        type: 'bar',
        height: 350,
        stacked: true,
        toolbar: {
            show: false
        },
        zoom: {
            enabled: true
        }
    },
    colors: [
        '#87B546', '#6C91BF',
    ],
    plotOptions: {
        bar: {
            horizontal: false,
            borderRadiusApplication: 'end',
            borderRadiusWhenStacked: 'last',
            dataLabels: {
                total: {
                    enabled: false
                }
            }
        }
    },
    dataLabels: {
        enabled: false
    },
    xaxis: {
        type: 'datetime',
        categories: [
            '2023-07-01',
            '2023-08-01',
        ]
    },
    legend: {
        show: false
    },
    fill: {
        opacity: 1
    }
})
</script>

<template>
    <div class="bg-grey">
        <div class="flex gap-8">
            <button class="green-btn">1Y</button>
            <button class="green-btn all-btn">All Time</button>
        </div>
        <h5 class="heading">Historical Performance</h5>
        <div class="max-w-1010 flex justify-space flex-col">
            <div class="mt-0">
                <p class="blue-text">7.29%</p>
                <p class="text-center" style="color: #404040;">IRR</p>
            </div>
            <div class="mt-0">
                <p class="blue-text">3.3%</p>
                <p class="text-center" style="color: #404040;">Avg Divided Yield</p>
            </div>
            <div class="mt-0">
                <p class="blue-text">6.3%</p>
                <p class="text-center" style="color: #404040;">Avg Appreciation</p>
            </div>
        </div>
        <div class="flex gap-18 justify-center flex-col">
            <div class="flex gap-8">
                <div class="green-box"></div>
                <p class="m-0">Dividend Yield </p>
            </div>
            <div class="flex gap-8">
                <div class="green-box bg-blue"></div>
                <p class="m-0">Appreciation </p>
            </div>
        </div>
        <div id="chart" style="max-width: 420px; width: 100%; margin: 0 auto; margin-top: 34px;">
            <ClientOnly>
                <VueApexCharts type="bar" height="350" :options="chartOptions" :series="series" />
            </ClientOnly>
        </div>
    </div>
</template>
<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');
*{
     font-family: "Poppins", sans-serif;
}

.text-center {
    text-align: center;
}

.gap-18 {
    gap: 18px;
}

.justify-center {
    justify-content: center;
}

.bg-blue {
    background-color: #6C91BF !important;
}

.green-box {
    width: 24px;
    height: 10px;
    background-color: #87B546;
}

.justify-space {
    justify-content: space-between;
}

.blue-text {
    color: #4B6585;
    font-weight: 800;
    font-size: 45px;
    margin: 0;

}

.max-w-1010 {
    max-width: 1010px;
    margin: 0 auto;
}

.heading {
    text-align: center;
    font-weight: 300;
    font-size: 14px;
    color: black;
    margin-bottom: 32px;
}

.all-btn {
    color: black !important;
    background: transparent !important;
}

.flex {
    display: flex;
    align-items: center;
}

.gap-8 {
    gap: 8px;
}

.green-btn {
    padding: 8px 32px;
    background-color: #6DA84D;
    border: 1px solid #6DA84D;
    color: white;
    border-radius: 8px;
    cursor: pointer;
}

.bg-grey {
    background-color: #F8F8F8;
    border-radius: 20px;
    padding: 35px;
    max-width: 1920px;
    margin: 0 auto 150px auto;
}

@media (max-width:768px) {
    .m-0 {
        margin: 0;
    }

    .bg-grey {
        background-color: #F8F8F8;
        border-radius: 20px;
        padding: 35px;
        margin-bottom: 50px;
    }
}

@media (max-width:576px) {
    .flex-col {
        flex-direction: column;
    }
}
</style>