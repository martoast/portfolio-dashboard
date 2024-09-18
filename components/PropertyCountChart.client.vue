<script setup>
import { ref, computed } from 'vue'

// Simulated data for property count over the last 12 months
const propertyHistory = ref([
  10, 10, 11, 11, 12, 12,
  13, 13, 14, 15, 15, 16
])

const currentCount = computed(() => propertyHistory.value[propertyHistory.value.length - 1])
const previousCount = computed(() => propertyHistory.value[propertyHistory.value.length - 2])
const countChange = computed(() => currentCount.value - previousCount.value)

const chartOptions = {
  chart: {
    type: 'line',
    height: 160,
    sparkline: {
      enabled: true
    },
  },
  stroke: {
    curve: 'straight',
    width: 2,
  },
  markers: {
    size: 4,
  },
  colors: ['#3B82F6'], // Tailwind CSS blue-500
  title: {
    text: currentCount.value.toString(),
    offsetX: 0,
    style: {
      fontSize: '24px',
      fontWeight: 'bold',
      color: '#374151', // Tailwind CSS gray-700
    }
  },
  subtitle: {
    text: `${countChange.value >= 0 ? '+' : '-'}${Math.abs(countChange.value)} property${countChange.value !== 1 ? 'ies' : ''}`,
    offsetX: 0,
    style: {
      fontSize: '14px',
      fontWeight: '600',
      color: '#059669',
    }
  }
}

const series = [{
  name: 'Properties',
  data: propertyHistory.value
}]
</script>

<template>
  <div class="bg-white rounded-lg shadow p-6">
    <h3 class="font-bold text-xl mb-2 text-gray-800">Total Properties</h3>
    <apexchart type="line" height="160" :options="chartOptions" :series="series"></apexchart>
    <p class="text-sm text-gray-600 mt-2">Property count over the past 12 months</p>
  </div>
</template>