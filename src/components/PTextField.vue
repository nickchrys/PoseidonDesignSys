<script setup>
import { ref } from 'vue'

defineProps({
    label: String,
    type: {
        type: String,
        default: 'text'
    },
    class: String
})

const input = ref('')
</script>

<template>
    <input :class="class" v-model="input" :type="type" :placeholder="label" />
</template>
