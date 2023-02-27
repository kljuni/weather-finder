<template>
    <div v-if="weatherData">
    <p v-if="feels_like">
        <v-icon icon="mdi-vuetify"></v-icon>
        feels_like: {{ feels_like }}
    </p>
    </div>
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
        feels_like() {
            if (this.weatherData) {
                return this.weatherData.main.feels_like
            } else null
        },
    }
}
</script>


