<script setup>

import { ref } from 'vue'

const props = defineProps({
    label: String,
    type: {
        type: String,
        default: 'text'
    },
    design: {
        type: String,
        default: 'default',
    }
})

const input = ref('')

</script>

<template>
    <input :class="design" v-model="input" :type="type" :placeholder="label" />
</template>
