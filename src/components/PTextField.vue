<script setup>

defineProps({
    label: String,
    type: {
        type: String,
        default: 'text'
    },
    class: String
})

</script>

<template>
    <input :class="class" v-model="input" :type="type" :placeholder="label" />
</template>
