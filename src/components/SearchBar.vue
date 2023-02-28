<template>
    <v-card class="my-5 mx-auto" max-width="400">
        <v-text-field v-model="citySearch" density="compact" variant="solo" label="Search cities"
            append-inner-icon="mdi-magnify" single-line hide-details
            @input="(event) => emit('input', event.target.value)"
            @keydown.enter.prevent="fetchMethod"
            @click:append-inner="fetchMethod"
            ></v-text-field>
    </v-card>
</template>


<script setup>
import { ref, watch } from 'vue'

let citySearch = ref("")

const props = defineProps({ searchText: String })
const emit = defineEmits(['input', 'request-city-wather']);

watch(() => props.searchText, (newValue) => {
    citySearch.value = newValue
})

const fetchMethod = function () {
    if (props.searchText.length > 0) {
        emit('request-city-wather', props.searchText)
    }
}
</script>
