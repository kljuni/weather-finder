<template>
    <v-card class="my-5 mx-auto" max-width="400">
        <div class="my-5 mx-3 text-left">
            <div class="text-h5 my-3">History</div>
            <v-chip v-for="item in props.history" :key="item.id" @click="retrieveDataFromHistory(item.id)" class="ma-1">
                {{ item.name }}
            </v-chip>
        </div>
    </v-card>
</template>


<script setup>
import { defineEmits } from 'vue'

const emit = defineEmits(['set-city-from-history']);

const props = defineProps({
    history: {
        type: Array,
        required: true
    }
})

const retrieveDataFromHistory = function (id) {
    emit('set-city-from-history', id)
}
</script>
