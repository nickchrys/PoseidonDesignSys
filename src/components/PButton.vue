<script setup>

defineProps({
  msg: String,
  class: String
})

</script>

<template>
    <button :class='class' type="button">{{ msg }}</button>
</template>

