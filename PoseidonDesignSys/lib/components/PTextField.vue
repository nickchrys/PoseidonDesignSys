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
    },
    maxlength: {
        type: Number,
        default: 400
    }
})

const input = ref('')

const base = 'p-textfield';
const giveDesign = computed(() => {
    const design = props.design !== 'default' ? `${base}--${props.design}` : base;
    return [design]
})

// Compute the remaining characters
const remainingCharacters = computed(() => {
    return props.maxlength - input.value.length;
})

</script>

<template>

    <template v-if="giveDesign.includes('p-textfield--textarea')">
        <div class="p-textfield__textarea-container">
            <textarea :class="giveDesign" v-model="input" :placeholder="label" :maxlength="maxlength"></textarea>
            <small class="p-textfield__char-counter">{{ remainingCharacters }} characters left</small>
        </div>
    </template>

    <template v-else>
        <input :class="giveDesign" v-model="input" :type="type" :placeholder="label" />
    </template>


</template>
