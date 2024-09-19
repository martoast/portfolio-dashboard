<script setup>
import { ref, computed } from 'vue'

// Simulated data for monthly cash flow over the last 12 months
const monthlyFlowHistory = ref([
  75000, 76000, 78000, 79000, 81000, 82000,
  84000, 85000, 87000, 89000, 91000, 93000
])

const currentMonthlyFlow = computed(() => monthlyFlowHistory.value[monthlyFlowHistory.value.length - 1])
const previousMonthlyFlow = computed(() => monthlyFlowHistory.value[monthlyFlowHistory.value.length - 2])
const monthlyFlowChange = computed(() => currentMonthlyFlow.value - previousMonthlyFlow.value)
const percentageChange = computed(() => (monthlyFlowChange.value / previousMonthlyFlow.value * 100).toFixed(2))

const yearlyFlow = computed(() => currentMonthlyFlow.value * 12)

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
    type: 'bar',
    height: 160,
    sparkline: {
      enabled: true
    },
  },
  plotOptions: {
    bar: {
      columnWidth: '80%'
    }
  },
  colors: ['#0058ff'], // Tailwind CSS green-500
  title: {
    text: formatCurrency(yearlyFlow.value),
    offsetX: 0,
    style: {
      fontSize: '24px',
      fontWeight: 'bold',
      color: '#374151', // Tailwind CSS gray-700
    }
  },
  subtitle: {
    text: `${monthlyFlowChange.value >= 0 ? '+' : '-'}${formatCurrency(Math.abs(monthlyFlowChange.value))} (${percentageChange.value}%) monthly`,
    offsetX: 0,
    style: {
      fontSize: '14px',
      fontWeight: '600',
      color: '#059669', // Tailwind CSS green-600
    }
  }
}

const series = [{
  name: 'Monthly Cash Flow',
  data: monthlyFlowHistory.value
}]
</script>

<template>
  <div class="bg-white rounded-lg shadow p-6">
    <h3 class="font-bold text-xl mb-2 text-gray-800">Estimated Yearly Cash Flow</h3>
    <apexchart type="bar" height="160" :options="chartOptions" :series="series"></apexchart>
    <p class="text-sm font-bold text-gray-600 mt-2">Monthly: {{ formatCurrency(currentMonthlyFlow) }}</p>
  </div>
</template>