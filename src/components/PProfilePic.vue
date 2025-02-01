<script setup>
import { defineProps } from 'vue'

const props = defineProps({
    profileImage: {
        type: String,
        //Go and make this transparent with 50x50
        default: 'src/assets/defaultPP.PNG'
    },
    class: String
});


</script>

<template>
    <div :class='props.class' type="button">
        <img :src="`${props.profileImage}`" @error="event => event.target.src = 'src/assets/defaultPP.PNG'">
    </div>
</template>
