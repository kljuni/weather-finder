<template>
    <v-container class="fill-height">
        <v-responsive class="d-flex align-start text-center fill-height">
            <SearchBar :searchText="citySearch" @input="updateSearchField" @request-city-wather="fetchCityWeather" />
            <HistoryList v-if="locationsHistory.length > 0" @set-city-from-history="setCityFromHistory"
                :history="locationsHistory" />
            <DisplayWeather :weatherData="selectedLocationWeather" :isLoading="isLoading" :isError="isError"
                :errorMessage="errorMessage" />
        </v-responsive>
    </v-container>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'
import SearchBar from '@/components/SearchBar.vue'
import HistoryList from '@/components/HistoryList.vue'
import DisplayWeather from '@/components/DisplayWeather.vue'

const citySearch = ref("")
const selectedLocationWeather = ref(null)
const locationsHistory = ref([])
const isLoading = ref(false)
const isError = ref(false)
const errorMessage = ref("")

const history = window.localStorage.getItem("history")

if (history != null) {
    locationsHistory.value = JSON.parse(history)
}

function updateSearchField(value) {
    citySearch.value = value
}

async function fetchCityWeather() {
    try {
        isLoading.value = true

        const query = `https://api.openweathermap.org/data/2.5/weather?q=${citySearch.value}&appid=${import.meta.env.VITE_WEATHERAPI}&units=metric`
        const weatherResponse = await axios.get(query)

        selectedLocationWeather.value = weatherResponse.data
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
    const existingDataIndex = locationsHistory.value.findIndex(element => element.id === weatherData.id)

    if (existingDataIndex > -1) {
        locationsHistory.value.splice(existingDataIndex, 1)
    }

    if (locationsHistory.value.length === 5) {
        locationsHistory.value.splice(4, 1)
    }

    locationsHistory.value.unshift(weatherData)
    
    const historyString = JSON.stringify(locationsHistory.value)
    window.localStorage.setItem("history", historyString)
}

function setCityFromHistory(id) {
    const historyData = locationsHistory.value.find(element => element.id === id)

    if (historyData) {
        updateSearchField(historyData.name)
        selectedLocationWeather.value = historyData        
    }
}

function handleCityRequestError(error) {
    errorMessage.value = error.response.statusText
    selectedLocationWeather.value = null
    isError.value = true

    setTimeout(() => {
        isError.value = false
        errorMessage.value = ""
    }, 10000)
}
</script>
