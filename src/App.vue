<script setup>
import { ref } from 'vue'

const lat = ref()
const long = ref()
const currentWeather = ref()
let loadingTag = true

const getWeather = async (latv, longv) => {
  loadingTag = false
  const weatherURL = ref(
    `https://api.open-meteo.com/v1/forecast?latitude=${latv}&longitude=${longv}&current_weather=true&temperature_unit=fahrenheit`
  )
  const res = await fetch(weatherURL.value)
  const data = await res.json()
  currentWeather.value = data
  loadingTag = true
}

const handleLosAngeles = () => {
  lat.value = 34.05
  long.value = -118.24
  getWeather(lat.value, long.value)
}
const handleNashville = () => {
  lat.value = 36.16
  long.value = -86.78
  getWeather(lat.value, long.value)
}
const handleNewYork = () => {
  lat.value = 40.71
  long.value = -74
  getWeather(lat.value, long.value)
}

const handleCurrent = async () => {
  const success = (pos) => {
    const here = pos.coords
    getWeather(here.latitude, here.longitude)
    lat.value = here.latitude
    long.value = here.longitude
  }
  await navigator.geolocation.getCurrentPosition(success)
}

const weatherReport = (val) => {
  let answer = ''
  switch (val) {
    case 0:
      answer = 'Clear sky'
      break
    case 1:
      answer = 'Mainly clear'
      break
    case 2:
      answer = 'Partly cloudy'
      break
    case 3:
      answer = 'Overcast'
      break
    case 45:
      answer = 'Fog'
      break
    case 48:
      answer = 'Freezing fog'
      break
    case 51:
      answer = 'Light Drizzle'
      break
    case 53:
      answer = 'Moderate Drizzle'
      break
    case 55:
      answer = 'Dense Drizzle'
      break
    case 56:
      answer = 'Light Freezing Drizzle'
      break
    case 57:
      answer = 'Heavy Freezing Drizzle'
      break
    case 61:
      answer = 'Slight Rain'
      break
    case 63:
      answer = 'Moderate Rain'
      break
    case 65:
      answer = 'Heavy Rain'
      break
    case 66:
      answer = 'Light Freezing Rain'
      break
    case 67:
      answer = 'Heavy Freezing Rain'
      break
    case 71:
      answer = 'Slight Snow Fall'
      break
    case 73:
      answer = 'Moderate Snow Fall'
      break
    case 75:
      answer = 'Heavy Snow Fall'
      break
    case 77:
      answer = 'Snow Grains'
      break
    case 80:
      answer = 'Slight Rain Showers'
      break
    case 81:
      answer = 'Moderate Rain Showers'
      break
    case 82:
      answer = 'Violent Rain Showers'
      break
    case 85:
      answer = 'Slight Snow Showers'
      break
    case 86:
      answer = 'Heavy Snow Showers'
      break
    case 95:
      answer = 'Thunderstorm: Slight or moderate'
      break
    case 96:
      answer = 'Thunderstorm with slight hail'
      break
    case 99:
      answer = 'Thunderstorm with heavy hail'
      break
  }
  return answer
}
// 0	Clear sky
// 1, 2, 3	Mainly clear, partly cloudy, and overcast
// 45, 48	Fog and depositing rime fog
// 51, 53, 55	Drizzle: Light, moderate, and dense intensity
// 56, 57	Freezing Drizzle: Light and dense intensity
// 61, 63, 65	Rain: Slight, moderate and heavy intensity
// 66, 67	Freezing Rain: Light and heavy intensity
// 71, 73, 75	Snow fall: Slight, moderate, and heavy intensity
// 77	Snow grains
// 80, 81, 82	Rain showers: Slight, moderate, and violent
// 85, 86	Snow showers slight and heavy
// 95 *	Thunderstorm: Slight or moderate
// 96, 99 *	Thunderstorm with slight and heavy hail
</script>

<template>
  <div>
    <h1>Get Weather</h1>
  </div>
  <div>
    <button @click="handleLosAngeles">Los Angeles</button>
    <button @click="handleNashville">Nashville</button>
    <button @click="handleNewYork">New York</button>
    <button @click="handleCurrent">Current Location</button>
  </div>
  <div>
    <div v-if="!loadingTag"><h1>Loading...</h1></div>
    <div v-if="currentWeather">
      <div>Latitude: {{ lat }}</div>
      <div>Longitude: {{ long }}</div>
      <div>Current Weather: {{ weatherReport(currentWeather.current_weather.weathercode) }}</div>
      <div>Current Temp: {{ currentWeather.current_weather.temperature }}</div>
    </div>
  </div>
</template>

<style scoped></style>
