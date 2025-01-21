<script setup>
import { ref } from 'vue'

defineProps({
  msg: String,
  class: String
})

const count = ref(0)
</script>

<template>
    <button :class='class' type="button">{{ msg }}</button>
</template>

