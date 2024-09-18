<script setup>
import { ref, computed, onMounted } from 'vue'

const propertyData = ref([
  { name: "Sunset Meadows Apartments", data: [280000, 310000, 350000, 425000, 500000, 580000] },
  { name: "Oakridge Townhomes", data: [320000, 350000, 390000, 460000, 520000, 610000] },
  { name: "Riverfront Condos", data: [400000, 450000, 520000, 600000, 680000, 750000] },
  { name: "Pine Valley Estates", data: [550000, 600000, 650000, 720000, 800000, 920000] },
  { name: "Maplewood Terrace", data: [230000, 260000, 300000, 350000, 410000, 480000] },
  { name: "Hillcrest Manor", data: [480000, 520000, 570000, 650000, 730000, 850000] }
])

const windowWidth = ref(1024) // Default to a larger screen size
const isMobile = computed(() => windowWidth.value < 768)

onMounted(() => {
  windowWidth.value = window.innerWidth
  window.addEventListener('resize', () => {
    windowWidth.value = window.innerWidth
  })
})

const chartOptions = computed(() => ({
  chart: {
    type: 'line',
    height: isMobile.value ? 400 : 350,
    zoom: { enabled: false },
  },
  colors: ['#0058ff', '#00d1ff', '#003380', '#64748b'],
  dataLabels: { enabled: false },
  stroke: { width: 3, curve: 'smooth' },
  title: {
    text: 'Sale Price History',
    align: 'bottom'
  },
  grid: {
    row: { colors: ['#e2e8f0', 'transparent'], opacity: 0.5 },
    borderColor: '#cbd5e1',
  },
  xaxis: {
    categories: ['2010', '2015', '2020', '2023'],
    labels: { style: { colors: '#64748b' } },
  },
  yaxis: {
    title: { text: 'Sale Price ($)', style: { color: '#1e293b' } },
    labels: {
      style: { colors: '#64748b' },
      formatter: (value) => new Intl.NumberFormat('en-US', {
        style: 'currency', currency: 'USD', minimumFractionDigits: 0, maximumFractionDigits: 0
      }).format(value)
    }
  },
  legend: {
    position: isMobile.value ? 'bottom' : 'bottom',
    offsetY: isMobile.value ? 0 : 25,
    offsetX: isMobile.value ? 0 : -5,
    labels: { colors: '#1e293b' },
  },
  tooltip: {
    theme: 'light',
    y: {
      formatter: (value) => new Intl.NumberFormat('en-US', {
        style: 'currency', currency: 'USD', minimumFractionDigits: 0, maximumFractionDigits: 0
      }).format(value)
    }
  }
}))
</script>

<template>
  <div class="chart-container">
    <client-only>
      <apexchart
        type="line"
        :height="isMobile ? 400 : 350"
        :options="chartOptions"
        :series="propertyData"
      />
    </client-only>
  </div>
</template>

<style scoped>
.chart-container {
  border-radius: 8px;
  padding: 16px;
}
</style>