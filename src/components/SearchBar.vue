<template>
    <v-text-field v-model="citySearch" density="compact" variant="solo" label="Search cities"
        append-inner-icon="mdi-magnify" single-line hide-details @keydown.enter.prevent="fetchMethod"
        @click:append-inner="fetchMethod"></v-text-field>
</template>


<script setup>
import { defineEmits, ref } from 'vue'

let citySearch = ref("")

const emit = defineEmits(['request-city-wather']);

const fetchMethod = function() {
    if (citySearch.value.length > 0) {
        emit('request-city-wather', citySearch)
    }    
}
</script>
