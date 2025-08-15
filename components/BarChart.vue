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
            show: true
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
    <div id="chart" style="max-width: 420px; width: 100%; margin: 0 auto;">
        <ClientOnly>
            <VueApexCharts type="bar" height="350" :options="chartOptions" :series="series" />
        </ClientOnly>
    </div>
</template>