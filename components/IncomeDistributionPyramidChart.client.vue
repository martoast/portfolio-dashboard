<script setup>
import { reactive, onMounted } from 'vue'

// Sample data for multiple properties
const properties = [
  { address: '123 Main St, San Diego, CA', demographics: { medianIncome: "128839" } },
  { address: '456 Elm St, Los Angeles, CA', demographics: { medianIncome: "94700" } },
  { address: '789 Oak St, San Francisco, CA', demographics: { medianIncome: "149600" } },
  { address: '321 Pine St, Sacramento, CA', demographics: { medianIncome: "91100" } },
  { address: '654 Maple St, San Jose, CA', demographics: { medianIncome: "168500" } },
  { address: '987 Birch Rd, Fresno, CA', demographics: { medianIncome: "70000" } },
  { address: '741 Cedar Ave, Oakland, CA', demographics: { medianIncome: "102000" } },
  { address: '852 Redwood Ln, Santa Barbara, CA', demographics: { medianIncome: "115000" } },
  { address: '369 Spruce St, Bakersfield, CA', demographics: { medianIncome: "65000" } },
  { address: '159 Willow Dr, Anaheim, CA', demographics: { medianIncome: "98000" } },
  { address: '654 Maple St, San Jose, CA', demographics: { medianIncome: "168500" } },
  { address: '987 Birch Rd, Fresno, CA', demographics: { medianIncome: "70000" } },
  { address: '741 Cedar Ave, Oakland, CA', demographics: { medianIncome: "102000" } },
  { address: '852 Redwood Ln, Santa Barbara, CA', demographics: { medianIncome: "115000" } },
  { address: '369 Spruce St, Bakersfield, CA', demographics: { medianIncome: "65000" } },
  { address: '159 Willow Dr, Anaheim, CA', demographics: { medianIncome: "98000" } },
]

const incomeCategories = [
  { name: 'Low Income', threshold: 80000, color: '#0058ff' },
  { name: 'Medium Income', threshold: 120000, color: '#00d1ff' },
  { name: 'High Income', threshold: Infinity, color: '#003380' },
]

const categorizeIncome = (income) => {
  const incomeValue = parseInt(income)
  return incomeCategories.find(category => incomeValue < category.threshold)
}

const getIncomeDistribution = () => {
  const distribution = Object.fromEntries(incomeCategories.map(cat => [cat.name, 0]))
  
  properties.forEach(property => {
    const category = categorizeIncome(property.demographics.medianIncome)
    distribution[category.name]++
  })

  return distribution
}

const chartConfig = reactive({
  chartOptions: {
    chart: {
      type: 'donut',
    },
    labels: incomeCategories.map(cat => 
      cat.threshold < Infinity 
        ? `${cat.name} (< $${cat.threshold.toLocaleString()})`
        : `${cat.name} (≥ $${incomeCategories[incomeCategories.length - 2].threshold.toLocaleString()})`
    ),
    title: {
      text: 'Property Demographics Income Distribution',
      align: 'left',
    },
    colors: incomeCategories.map(cat => cat.color),
    legend: {
      position: 'bottom',
    },
    plotOptions: {
      pie: {
        donut: {
          labels: {
            show: true,
            total: {
              show: true,
              label: 'Total Properties',
              formatter: function (w) {
                return w.globals.seriesTotals.reduce((a, b) => a + b, 0)
              }
            }
          }
        }
      }
    },
    tooltip: {
      y: {
        formatter: function(value) {
          return value + ' properties'
        }
      }
    }
  },
  series: []
});

onMounted(() => {
  const distribution = getIncomeDistribution()
  chartConfig.series = incomeCategories.map(cat => distribution[cat.name])
})
</script>

<template>
  <div class="chart-container">
    <apexchart
      height="400"
      type="donut"
      :options="chartConfig.chartOptions"
      :series="chartConfig.series"
    ></apexchart>
  </div>
</template>

<style scoped>
.chart-container {
  min-height: 300px;
}
</style>