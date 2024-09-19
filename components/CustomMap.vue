<template>
    <div class="box bg-white rounded-lg shadow" id="map" style="height:450px; width: auto;"></div>
  </template>
  
  <script setup>
  const nuxtApp = useNuxtApp()
  const config = useRuntimeConfig()
  const mapboxgl = nuxtApp.mapboxgl
  const access_token = config.public.MAPBOX_API_TOKEN
  
  let map = {}
  
  const map_config = {
    style: "mapbox://styles/mapbox/streets-v12",
    zoom: 0,
    pitch: 0,
    bearing: 0,
    center: [-100.486052, 37.830348], // Centered on San Diego
  }


  
  const properties = ref([
    // San Diego properties
    {
      address: "123 Ocean View Dr, San Diego, CA",
      bedrooms: 3,
      bathrooms: 2,
      sqft: 1800,
      estimatedValue: 850000,
      price: 825000,
      latitude: 32.7157,
      longitude: -117.1611
    },
    {
      address: "456 Balboa Park Ln, San Diego, CA",
      bedrooms: 4,
      bathrooms: 3,
      sqft: 2200,
      estimatedValue: 1100000,
      price: 1050000,
      latitude: 32.7341,
      longitude: -117.1442
    },
    {
      address: "789 Coronado Beach Rd, San Diego, CA",
      bedrooms: 2,
      bathrooms: 2,
      sqft: 1500,
      estimatedValue: 950000,
      price: 925000,
      latitude: 32.6859,
      longitude: -117.1831
    },
    {
      address: "101 Gaslamp Quarter St, San Diego, CA",
      bedrooms: 1,
      bathrooms: 1,
      sqft: 900,
      estimatedValue: 550000,
      price: 535000,
      latitude: 32.7098,
      longitude: -117.1600
    },
    {
      address: "202 La Jolla Cove Way, San Diego, CA",
      bedrooms: 5,
      bathrooms: 4,
      sqft: 3500,
      estimatedValue: 2500000,
      price: 2450000,
      latitude: 32.8328,
      longitude: -117.2713
    },
    // Los Angeles properties
    {
      address: "303 Hollywood Blvd, Los Angeles, CA",
      bedrooms: 2,
      bathrooms: 2,
      sqft: 1200,
      estimatedValue: 750000,
      price: 735000,
      latitude: 34.1016,
      longitude: -118.3267
    },
    {
      address: "404 Venice Beach Ave, Los Angeles, CA",
      bedrooms: 3,
      bathrooms: 2,
      sqft: 1600,
      estimatedValue: 1200000,
      price: 1175000,
      latitude: 33.9850,
      longitude: -118.4695
    },
    {
      address: "505 Downtown LA St, Los Angeles, CA",
      bedrooms: 1,
      bathrooms: 1,
      sqft: 800,
      estimatedValue: 600000,
      price: 585000,
      latitude: 34.0407,
      longitude: -118.2468
    },
    {
      address: "606 Beverly Hills Dr, Los Angeles, CA",
      bedrooms: 6,
      bathrooms: 5,
      sqft: 4500,
      estimatedValue: 5000000,
      price: 4950000,
      latitude: 34.0736,
      longitude: -118.4004
    },
    {
      address: "707 Santa Monica Pier Rd, Los Angeles, CA",
      bedrooms: 2,
      bathrooms: 2,
      sqft: 1400,
      estimatedValue: 1100000,
      price: 1075000,
      latitude: 34.0089,
      longitude: -118.4973
    },
    // San Antonio properties
    {
      address: "808 Alamo Plaza, San Antonio, TX",
      bedrooms: 3,
      bathrooms: 2,
      sqft: 1800,
      estimatedValue: 350000,
      price: 340000,
      latitude: 29.4260,
      longitude: -98.4861
    },
    {
      address: "909 River Walk Way, San Antonio, TX",
      bedrooms: 2,
      bathrooms: 2,
      sqft: 1500,
      estimatedValue: 400000,
      price: 390000,
      latitude: 29.4241,
      longitude: -98.4936
    },
    {
      address: "1010 Pearl Brewery St, San Antonio, TX",
      bedrooms: 1,
      bathrooms: 1,
      sqft: 900,
      estimatedValue: 250000,
      price: 245000,
      latitude: 29.4435,
      longitude: -98.4799
    },
    {
      address: "1111 King William District Rd, San Antonio, TX",
      bedrooms: 4,
      bathrooms: 3,
      sqft: 2500,
      estimatedValue: 550000,
      price: 535000,
      latitude: 29.4128,
      longitude: -98.4935
    },
    {
      address: "1212 San Antonio Missions Way, San Antonio, TX",
      bedrooms: 3,
      bathrooms: 2,
      sqft: 1700,
      estimatedValue: 300000,
      price: 295000,
      latitude: 29.3814,
      longitude: -98.4790
    }
  ])
  
  const statesWithProperties = ["06", "48"] // California and Texas

const initMap = () => {
  mapboxgl.accessToken = access_token

  map_config.zoom = 3

  map = new mapboxgl.Map({
    container: "map",
    style: map_config.style,
    zoom: map_config.zoom,
    pitch: map_config.pitch,
    bearing: map_config.bearing,
    center: map_config.center,
    dragPan: true,
    antialias: true,
  })

  map.on('load', () => {
    addStateLayer()
    addPropertyMarkers()
  })
}

const addStateLayer = () => {
  map.addSource('states', {
    'type': 'geojson',
    'data': 'https://docs.mapbox.com/mapbox-gl-js/assets/us_states.geojson'
  })

  map.addLayer({
    'id': 'state-fills',
    'type': 'fill',
    'source': 'states',
    'layout': {},
    'paint': {
      'fill-color': [
        'match',
        ['get', 'STATE_ID'],
        ...statesWithProperties.flatMap(id => [id, '#0058ff']),
        '#FFFFFF'
      ],
      'fill-opacity': 0.3
    }
  })

  map.addLayer({
    'id': 'state-borders',
    'type': 'line',
    'source': 'states',
    'layout': {},
    'paint': {
      'line-color': '#627BC1',
      'line-width': 2
    }
  })
}

const addPropertyMarkers = () => {
  properties.value.forEach(property => {
    new mapboxgl.Marker({ color: '#0058ff' })
      .setLngLat([property.longitude, property.latitude])
      .setPopup(
        new mapboxgl.Popup({ offset: 25 })
          .setHTML(`
            <div class="p-2">
              <h3 class="font-bold text-sm mb-1">${property.address}</h3>
              <p class="text-xs">Beds: ${property.bedrooms}, Baths: ${property.bathrooms}</p>
              <p class="text-xs">Sqft: ${property.sqft}</p>
              <p class="text-xs">Est. Value: $${property.estimatedValue.toLocaleString()}</p>
              <p class="text-xs">Price: $${property.price.toLocaleString()}</p>
              <a href="https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(property.address)}" 
                 target="_blank" 
                 rel="noopener noreferrer" 
                 class="mt-2 inline-block px-4 py-2 bg-blue-500 text-white text-xs font-bold rounded hover:bg-blue-300 transition-colors">
                View on Google Maps
              </a>
            </div>
          `)
      )
      .addTo(map)
  })
}

onMounted(() => {
  initMap()
})
  </script>