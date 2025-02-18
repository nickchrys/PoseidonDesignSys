<script setup>

import { ref, computed } from 'vue'

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

const base = 'p-textfield';
const giveDesign = computed(() => {
    const design = props.design !== 'default' ? `${base}--${props.design}` : base;
    return [design]
})

</script>

<template>
    <input :class="giveDesign" v-model="input" :type="type" :placeholder="label" />
</template>
