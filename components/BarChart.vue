<script setup lang="ts">
import { ref } from 'vue'
import VueApexCharts from 'vue3-apexcharts'

const series = ref([{
    name: 'PRODUCT A',
    data: [1, 5, 1, 4, 1, 3, 1, 2, 1, 6, 1]
}, {
    name: 'PRODUCT B',
    data: [10, 13, 8, 10, 6, 8, 8, 9, 10, 14, 9],
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
            '2023-09-01',
            '2023-10-01',
            '2023-11-01',
            '2023-12-01',
            '2024-01-01',
            '2024-02-01',
            '2024-03-01',
            '2024-04-01',
            '2024-05-01'
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
    <div id="chart">
        <ClientOnly>
            <VueApexCharts type="bar" height="350" :options="chartOptions" :series="series" />
        </ClientOnly>
    </div>
</template>