<script setup>

import { ref, computed } from 'vue';

const props = defineProps({
    profileImage: {
        type: String,
        //Go and make this transparent with 50x50
        default: 'Poseidon-Design-System/PoseidonDesignSys/lib/assets/img/defaultPP.png'
    },
    design: {
        type: String,
        default: 'default',
    }
});


//Come back to this to see if you can make it faster with preload or something
const imageError = ref(false)

const validatedImage = computed(() => {
    if (imageError.value || !props.profileImage) {
        return new URL('../assets/img/defaultPP.png', import.meta.url).href;
    }
    return props.profileImage;
});

const handleImageError = () => {
    imageError.value = true;
}
const base = 'p-profilepic';
const giveDesign = computed(() => {
    const design = props.design !== 'default' ? `${base}--${props.design}` : base;
    return [design]
})

</script>

<template>
    <div :class='giveDesign' type="button">
        <img :src="validatedImage" @error="handleImageError" />
    </div>
</template>
