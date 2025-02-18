<script setup>

import { ref, computed } from 'vue'
import { PButton } from './index'

const emit = defineEmits(['option-selected']);

const props = defineProps({
    dropDownLabel: {
        type: String,
        required: true
    },
    options: {
        type: Array,
        required: true,
        validator: (value) => value.every(item => typeof item === 'string')
    },
    design: {
        type: String,
        default: '',
        validator: (value) => ['event', 'flight'].includes(value)
    }
});

const base = 'p-dropdown';
const giveDesign = computed(() => {
    const design = props.design !== 'default' ? `${base}--${props.design}` : base;
    return design
})

const dropDown = ref(false);
const dropDownLabel = ref(props.dropDownLabel);

const toggleDropDown = () => {
    dropDown.value = !dropDown.value;
}

const selectOption = (option) => {
    dropDown.value = false;
    dropDownLabel.value = option;
    emit('option-selected', option);
};
</script>

<template>

    <div :class="giveDesign">
        <div class="dropdown-trigger" @click="toggleDropDown">
            <PButton design="dropdown" :label="dropDownLabel"></PButton>
            <svg :class="`${giveDesign}__icon`" xmlns="http://www.w3.org/2000/svg" width="9" height="4"
                viewBox="0 0 9 4">
                <path fill="currentColor" d="M4.5 4L0 0h9L4.5 4z" />
            </svg>
        </div>
        <ul v-if="dropDown" :class="`${giveDesign}__menu`">
            <li v-for="option in options" :value="option" @click="selectOption(option)">
                {{ option }}
            </li>
        </ul>
    </div>
</template>