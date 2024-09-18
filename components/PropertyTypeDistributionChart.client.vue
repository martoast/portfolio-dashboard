<script setup>
import { reactive } from 'vue'

const chartConfig = reactive({
  chartOptions: {
    chart: {
      type: 'donut',
    },
    labels: ['Single Family', 'Multi Family', 'Condo', 'Townhouse', 'Commercial'],
    title: {
      text: 'Property Type Distribution',
      align: 'bottom',
    },
    colors: ['#0058ff', '#00d1ff', '#003380', '#64748b', '#1e293b'],
    legend: {
      position: 'bottom',
      offsetY: 5,
    
    },
  },
  series: [45, 20, 15, 12, 8]
});
</script>

<template>
  <div>
    <apexchart
      height="400"
      type="donut"
      :options="chartConfig.chartOptions"
      :series="chartConfig.series"
    ></apexchart>
  </div>
</template>
