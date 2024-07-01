<script setup lang="ts">
import { ref } from 'vue'

const latitude = ref<number | null>(null)
const longitude = ref<number | null>(null)

const getLocation = () => {
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(position => {
      latitude.value = position.coords.latitude
      longitude.value = position.coords.longitude
    }, error => {
      console.error('Error getting location:', error)

      // Handle error if necessary
    })
  }
  else {
    console.error('Geolocation is not supported by this browser.')

    // Handle lack of geolocation support
  }
}
</script>

<template>
  <div>
    <VBtn
      prepend-icon="ri-navigation-fill"
      @click="getLocation"
    >
      GPS Location
    </VBtn>
    <div
      v-if="latitude && longitude"
      class="location"
    >
      <p> Latitude: {{ latitude }}</p>
      <p> Longitude: {{ longitude }}</p>
    </div>
  </div>
</template>

<style>
.location{
  margin-block-start: 3%;
}

.location p{
  font-size: 1rem;
  font-weight: bold;
}
</style>
