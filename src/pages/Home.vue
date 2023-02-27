<template>
    <v-container class="fill-height">
        <v-responsive class="d-flex align-start text-center fill-height">
            <v-card 
                class="mx-auto"
                max-width="400">
                    <SearchBar  @request-city-wather="fetchCityWeather" />
                    <v-progress-circular v-if="isLoading" indeterminate color="primary"></v-progress-circular>
                    <DisplayWeather v-else :weatherData="displayedWeatherData"/>
            </v-card>
        </v-responsive>
    </v-container>
</template>

<script setup>
    import { ref } from 'vue'
    import axios from 'axios'
    import SearchBar from '@/components/SearchBar.vue'
    import DisplayWeather from '@/components/DisplayWeather.vue'

    const displayedWeatherData = ref(null)
    const isLoading = ref(false) 

    async function fetchCityWeather(city) {
        isLoading.value = true
        const query = `https://api.openweathermap.org/data/2.5/weather?q=${city.value}&appid=${import.meta.env.VITE_WEATHERAPI}`
        const weatherResponse = await axios.get(query)
        
        console.log(weatherResponse.data)
        isLoading.value = false
        displayedWeatherData.value = weatherResponse.data
    }
</script>
