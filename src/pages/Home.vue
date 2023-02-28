<template>
    <v-container class="fill-height">
        <v-responsive class="d-flex align-start text-center fill-height">
            <SearchBar @request-city-wather="fetchCityWeather" />
            <HistoryList v-if="weatherDataHistory.length > 0" @set-city-from-history="setCityFromHistory"
                :history="weatherDataHistory" />

            <v-progress-circular v-if="isLoading" indeterminate color="primary"></v-progress-circular>
            <DisplayWeather v-else-if="displayedWeatherData" :weatherData="displayedWeatherData" />
            <v-alert v-if="isError" :text="errorMessage" color="orange" variant="tonal" class="my-5 mx-auto" dense
                width="400"></v-alert>
        </v-responsive>
    </v-container>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'
import SearchBar from '@/components/SearchBar.vue'
import HistoryList from '@/components/HistoryList.vue'
import DisplayWeather from '@/components/DisplayWeather.vue'

const displayedWeatherData = ref(null)
const weatherDataHistory = ref([])
const isLoading = ref(false)
const isError = ref(false)
const errorMessage = ref("")

const history = window.localStorage.getItem("history")

if (history != null) {
    weatherDataHistory.value = JSON.parse(history)
}

async function fetchCityWeather(city) {
    try {
        isLoading.value = true
        const query = `https://api.openweathermap.org/data/2.5/weather?q=${city.value}&appid=${import.meta.env.VITE_WEATHERAPI}&units=metric`
        const weatherResponse = await axios.get(query)
        displayedWeatherData.value = weatherResponse.data
        isError.value = false
        addWeatherDataToHistory(weatherResponse.data)
        return true
    } catch (error) {
        handleCityRequestError(error)
        return false
    } finally {
        isLoading.value = false
    }
}

function addWeatherDataToHistory(weatherData) {
    const existingDataIndex = weatherDataHistory.value.findIndex(element => element.id === weatherData.id)

    if (existingDataIndex > 0) {
        weatherDataHistory.value.splice(existingDataIndex, 1)
    }

    if (weatherDataHistory.value.length === 5) {
        weatherDataHistory.value.splice(4, 1)
    }

    weatherDataHistory.value.unshift(weatherData)
    const historyString = JSON.stringify(weatherDataHistory.value)
    window.localStorage.setItem("history", historyString)
}

function setCityFromHistory(id) {
    const historyData = weatherDataHistory.value.find(element => element.id === id)
    if (historyData) {
        displayedWeatherData.value = historyData
    }
}

function handleCityRequestError(error) {
    errorMessage.value = error.response.statusText
    displayedWeatherData.value = null
    isError.value = true

    setTimeout(() => {
        isError.value = false
        errorMessage.value = ""
    }, 10000)
}
</script>
