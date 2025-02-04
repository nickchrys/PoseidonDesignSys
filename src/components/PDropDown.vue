<script setup>
import { ref } from 'vue'
import PButton from './PButton.vue';

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
    default: 'default',
  }
});

const dropDown = ref(false);
const dropDownLabel = ref(props.dropDownLabel);

const toggleDropDown = () => {
    dropDown.value = !dropDown.value;
}

const selectOption = (option) => {
    dropDown.value = false;
    dropDownLabel.value = option;
};
</script>

<template>

    <div :class="design">
        <div :class="design" @click="toggleDropDown">
            <PButton design="dropdown" :label="dropDownLabel"></PButton>
            <svg class='p-dropdown__icon' xmlns="http://www.w3.org/2000/svg" width="9" height="4" viewBox="0 0 9 4">
                <path fill="currentColor" d="M4.5 4L0 0h9L4.5 4z" />
            </svg>
        </div>
        <ul v-if="dropDown" class="p-dropdown__menu">
            <li v-for="option in options" @click="selectOption(option)">
                {{ option }}
            </li>
        </ul>
    </div>
</template>