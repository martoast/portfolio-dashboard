<script setup>
import { reactive, onMounted } from 'vue'

// Sample data for multiple properties
const properties = [
  {
    address: '123 Main St, San Diego, CA',
    demographics: {
      medianIncome: "128839",
      hudAreaName: "San Diego-Carlsbad, CA MSA"
    }
  },
  {
    address: '456 Elm St, Los Angeles, CA',
    demographics: {
      medianIncome: "94700",
      hudAreaName: "Los Angeles-Long Beach-Anaheim, CA MSA"
    }
  },
  {
    address: '789 Oak St, San Francisco, CA',
    demographics: {
      medianIncome: "149600",
      hudAreaName: "San Francisco-Oakland-Berkeley, CA MSA"
    }
  },
  {
    address: '321 Pine St, Sacramento, CA',
    demographics: {
      medianIncome: "91100",
      hudAreaName: "Sacramento-Roseville-Folsom, CA MSA"
    }
  },
  {
    address: '654 Maple St, San Jose, CA',
    demographics: {
      medianIncome: "168500",
      hudAreaName: "San Jose-Sunnyvale-Santa Clara, CA MSA"
    }
  }
]

const chartConfig = reactive({
  chartOptions: {
    chart: {
      type: 'bar',
      height: 350
    },
    plotOptions: {
      bar: {
        horizontal: true,
      }
    },
    dataLabels: {
      enabled: false
    },
    xaxis: {
      categories: [],
      title: {
        text: 'Median Income ($)'
      }
    },
    yaxis: {
      title: {
        text: 'Property Location'
      }
    },
    title: {
      text: 'Income Distribution Across Properties',
      align: 'left'
    },
    tooltip: {
      y: {
        formatter: function (val, { dataPointIndex }) {
          return `${properties[dataPointIndex].demographics.hudAreaName}: $${val.toLocaleString()}`
        }
      }
    }
  },
  series: [{
    name: 'Median Income',
    data: []
  }]
})

onMounted(() => {
  chartConfig.chartOptions.xaxis.categories = properties.map(p => p.address.split(',')[0])
  chartConfig.series[0].data = properties.map(p => parseInt(p.demographics.medianIncome))
})
</script>

<template>
  <div class="chart-container">
    <apexchart
      type="bar"
      :options="chartConfig.chartOptions"
      :series="chartConfig.series"
    ></apexchart>
  </div>
</template>

<style scoped>
.chart-container {
  min-height: 400px;
}
</style>