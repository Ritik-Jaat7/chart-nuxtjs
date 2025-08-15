<script setup>
import BarChart from "../components/BarChart.vue"
import LineChart from "../components/LineChart.vue"
</script>

<template>
  <div class="app-container">
    <h1 class="text-3xl font-bold underline mb-8">
      Chart Dashboard
    </h1>
    <BarChart />
    <LineChart />
  </div>
</template>


<style scoped>
.app-container {
  padding: 2rem;
  max-width: 1400px;
  margin: 0 auto;
}
</style>
