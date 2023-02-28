<template>
    <v-card class="mx-auto" max-width="400">
        <div class="text-h5 ma-3 text-left">{{ locatioName }}</div>
        <div class="my-6">
            <v-icon icon="mdi-thermometer"></v-icon> Temperature: {{ temp }} °C
        </div>
        <div class="my-6">
            <v-icon icon="mdi-run-fast"></v-icon> Feels like: {{ feels_like }} °C
        </div>
        <div class="my-6">
            <v-icon icon="mdi-thermometer-low"></v-icon> Temp. min: {{ temp_min }} °C
        </div>
        <div class="my-6">
            <v-icon icon="mdi-thermometer-high"></v-icon> Temp. max: {{ temp_max }} °C
        </div>
        <div class="my-6">
            <v-icon icon="mdi-water-percent"></v-icon> Humidity: {{ humidity }} %
        </div>
    </v-card>
</template>

<script>
export default {
    props: {
        weatherData: {
            type: Object,
            required: true,
        }
    },

    computed: {
        locatioName() {
            return this.weatherData.name
        },
        temp() {
            return this.weatherData.main.temp
        },
        feels_like() {
            return this.weatherData.main.feels_like
        },
        temp_min() {
            return this.weatherData.main.temp_min
        },
        temp_max() {
            return this.weatherData.main.temp_max
        },
        humidity() {
            return this.weatherData.main.humidity
        },
    }
}
</script>


