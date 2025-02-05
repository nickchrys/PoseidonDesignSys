<script setup>

import { computed } from 'vue';

const props = defineProps({
    profileImage: {
        type: String,
        //Go and make this transparent with 50x50
        default: 'src/assets/defaultPP.PNG'
    },
    design: {
        type: String,
        default: 'default',
    }
});

const base = 'p-profilepic';
const giveDesign = computed(() => {
    const design = props.design !== 'default' ? `${base}--${props.design}` : base;
    return [design]
})

</script>

<template>
    <div :class='giveDesign' type="button">
        <img :src="`${profileImage}`" @error="event => event.target.src = 'src/assets/defaultPP.PNG'">
    </div>
</template>
