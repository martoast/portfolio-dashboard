<template>
  <div class="px-4 sm:px-6 lg:px-8">
    <div class="sm:flex sm:items-center">
      <div class="sm:flex-auto">
        <h1 class="text-base font-semibold leading-6 text-gray-900">Properties</h1>
      </div>
      <!-- <div class="mt-4 sm:ml-16 sm:mt-0 sm:flex-none">
        <button type="button" class="block rounded-md bg-primary px-3 py-2 text-center text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-primary">Add property</button>
      </div> -->
    </div>
    <div class="-mx-4 mt-8 sm:-mx-0">
      <table class="min-w-full divide-y divide-gray-300">
        <thead>
          <tr>
            <th scope="col" class="py-3.5 pl-4 pr-3 text-left text-sm font-semibold text-gray-900 sm:pl-0">Address</th>
            <th scope="col" class="hidden px-3 py-3.5 text-left text-sm font-semibold text-gray-900 sm:table-cell">Bedrooms</th>
            <th scope="col" class="hidden px-3 py-3.5 text-left text-sm font-semibold text-gray-900 sm:table-cell">Bathrooms</th>
            <th scope="col" class="hidden px-3 py-3.5 text-left text-sm font-semibold text-gray-900 lg:table-cell">Year Built</th>
            <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">Sq Ft</th>
            <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">Estimated Value</th>
          </tr>
        </thead>
        <tbody class="divide-y divide-gray-200 bg-white">
          <tr v-for="property in properties" :key="property.address">
            <td class="w-full max-w-0 py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:w-auto sm:max-w-none sm:pl-0">
              {{ property.address }}
              <dl class="font-normal sm:hidden">
                <dt class="sr-only">Bedrooms</dt>
                <dd class="mt-1 truncate text-gray-700">{{ property.bedrooms }} bed</dd>
                <dt class="sr-only">Bathrooms</dt>
                <dd class="mt-1 truncate text-gray-500">{{ property.bathrooms }} bath</dd>
              </dl>
            </td>
            <td class="hidden px-3 py-4 text-sm text-gray-500 sm:table-cell">{{ property.bedrooms }}</td>
            <td class="hidden px-3 py-4 text-sm text-gray-500 sm:table-cell">{{ property.bathrooms }}</td>
            <td class="hidden px-3 py-4 text-sm text-gray-500 lg:table-cell">{{ property.yearBuilt }}</td>
            <td class="px-3 py-4 text-sm text-gray-500">{{ property.sqft }}</td>
            <td class="px-3 py-4 text-sm text-gray-500">${{ property.estimatedValue.toLocaleString() }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
const properties = [
  { 
    address: '123 Main St, Anytown, USA 12345', 
    bedrooms: 3, 
    bathrooms: 2, 
    yearBuilt: 1998, 
    sqft: 1800,
    estimatedValue: 320000
  },
  { 
    address: '456 Elm Ave, Somewhere City, USA 67890', 
    bedrooms: 4, 
    bathrooms: 3.5, 
    yearBuilt: 2005, 
    sqft: 2500,
    estimatedValue: 450000
  },
  { 
    address: '789 Oak Rd, Another Place, USA 13579', 
    bedrooms: 2, 
    bathrooms: 1, 
    yearBuilt: 1975, 
    sqft: 1200,
    estimatedValue: 180000
  },
  { 
    address: '321 Pine Ln, Newtown, USA 24680', 
    bedrooms: 5, 
    bathrooms: 4, 
    yearBuilt: 2020, 
    sqft: 3500,
    estimatedValue: 750000
  },
  { 
    address: '654 Cedar Blvd, Old City, USA 97531', 
    bedrooms: 3, 
    bathrooms: 2.5, 
    yearBuilt: 1985, 
    sqft: 2000,
    estimatedValue: 380000
  }
]
</script>