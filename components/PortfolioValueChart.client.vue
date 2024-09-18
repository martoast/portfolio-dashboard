<script setup>
import { ref, computed } from 'vue'

// Simulated data for portfolio value over the last 12 months, showing growth to ~15 million
const portfolioHistory = ref([
  13500000, 13650000, 13800000, 14000000, 14150000, 14300000,
  14450000, 14600000, 14750000, 14850000, 14950000, 15100000
])

const currentValue = computed(() => portfolioHistory.value[portfolioHistory.value.length - 1])
const previousValue = computed(() => portfolioHistory.value[portfolioHistory.value.length - 2])
const valueChange = computed(() => currentValue.value - previousValue.value)
const percentageChange = computed(() => (valueChange.value / previousValue.value * 100).toFixed(2))

const formatCurrency = (value) => {
  return new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD',
    minimumFractionDigits: 0,
    maximumFractionDigits: 0,
  }).format(value)
}

const chartOptions = {
  chart: {
    type: 'area',
    height: 160,
    sparkline: {
      enabled: true
    },
  },
  stroke: {
    curve: 'smooth',
    width: 2,
  },
  fill: {
    opacity: 0.3,
    type: 'gradient',
    gradient: {
      shadeIntensity: 1,
      opacityFrom: 0.7,
      opacityTo: 0.3,
    }
  },
  yaxis: {
    min: 13000000,
    max: 15500000,
  },
  colors: ['#0058ff'], // Tailwind CSS green-500
  title: {
    text: formatCurrency(currentValue.value),
    offsetX: 0,
    style: {
      fontSize: '24px',
      fontWeight: 'bold',
      color: '#374151', // Tailwind CSS gray-700
    }
  },
  subtitle: {
    text: `${valueChange.value >= 0 ? '+' : '-'}${formatCurrency(Math.abs(valueChange.value))} (${percentageChange.value}%)`,
    offsetX: 0,
    style: {
      fontSize: '14px',
      fontWeight: '600',
      color: '#059669', // Tailwind CSS green-600
    }
  }
}

const series = [{
  name: 'Portfolio Value',
  data: portfolioHistory.value
}]
</script>

<template>
  <div class="bg-white rounded-lg shadow p-6">
    <h3 class="font-bold text-xl mb-2 text-gray-800">Total Portfolio Value</h3>
    <apexchart type="area" height="160" :options="chartOptions" :series="series"></apexchart>
    <p class="text-sm text-gray-600 mt-2">Showing portfolio growth over the past 12 months</p>
  </div>
</template>