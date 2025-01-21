<script setup>
import { ref } from 'vue'

defineProps({
    label: String,
    organization: {
        organization: String,
        default: 'organization'
    },
    event: {
        event: String,
        default: 'event'
    },
    startDate: {
        startDate: Date(),
        default: 'startDate'
    },
    class: String
})

const input = ref('')
</script>

<template>
    <input :class="class" v-model="input" :type="type" :placeholder="label" />
</template>