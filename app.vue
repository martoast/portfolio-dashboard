<template>
  <div class="min-h-screen bg-gray-100">
    <div class="container mx-auto px-4 py-8">
      <div class="main">
        <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-8">
          <div class="box bg-white rounded-lg shadow p-4">
            <PropertyCountChart />
          </div>
          <div class="box bg-white rounded-lg shadow p-4">
            <PortfolioValueChart />
          </div>
          <div class="box bg-white rounded-lg shadow p-4">
            <CashFlowChart />
          </div>
        </div>

        <div class="grid grid-cols-1 gap-4 mb-8">
          
          <CustomMap/>
        
        </div>

        <div class="grid grid-cols-1 gap-4 mb-8">
          <div class="box bg-white rounded-lg shadow p-4">
            <PropertiesTable/>
          </div>
        </div>

        <div class="grid grid-cols-1 gap-4 mb-8">
          <div class="box bg-white rounded-lg shadow p-4">
            <div id="sale-price"><SalePriceHistoryChart /></div>
          </div>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-8">


          <div class="box bg-white rounded-lg shadow p-4">
            <div id="line"><PropertyTypeDistributionChart /></div>
          </div>

          <div class="box bg-white rounded-lg shadow p-4">
            <div id="income-line"><IncomeDistributionChart/></div>
          </div>

          <div class="box bg-white rounded-lg shadow p-4">
            <div id="Income donut"><IncomeDistributionPyramidChart/></div>
          </div>
          
          
        </div>

      </div>
    </div>
  </div>
</template>

<!-- <script setup>

import data from '/public/data.json'

console.log(data)

</script> -->