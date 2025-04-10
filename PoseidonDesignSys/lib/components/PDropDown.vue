<script setup>
import { ref, computed, watch } from 'vue'
import { PButton } from './index'

const props = defineProps({
    modelValue: {
        type: [String, Number],
        default: ''
    },
    dropDownLabel: {
        type: String,
        required: true
    },
    options: {
        type: Array,
        required: true,
        validator: (value) =>
            value.every(
                item =>
                    typeof item === 'string' ||
                    (typeof item === 'object' && item !== null && 'label' in item)
            )
    },
    design: {
        type: String,
        default: '',
        validator: (value) => ['event', 'flight', 'login'].includes(value)
    },
    description: {
        type: String,
        default: ''
    }
});

const emit = defineEmits(['update:modelValue', 'option-selected']);

const base = 'p-dropdown';
const giveDesign = computed(() => props.design !== 'default' ? `${base}--${props.design}` : base);

const dropDown = ref(false);
const internalLabel = ref(props.dropDownLabel);

const toggleDropDown = () => {
    dropDown.value = !dropDown.value;
}

const selectOption = (field, option) => {
    dropDown.value = false;
    internalLabel.value = typeof option === 'object' ? option.label : option;
    emit('update:modelValue', typeof option === 'object' ? option.value : option);
    emit('option-selected', { field, option });
}

// Watch for external changes in v-model
watch(
    () => props.modelValue,
    (newVal) => {
        const selected = props.options.find(
            o => typeof o === 'object' ? o.value === newVal : o === newVal
        );
        if(selected) {
            internalLabel.value = typeof selected === 'object' ? selected.label : selected;
        } else {
            internalLabel.value = props.dropDownLabel;
        }
    },
    { immediate: true }
);
</script>

<template>
    <div :class="giveDesign">
        <div class="dropdown-trigger" @click="toggleDropDown">
            <PButton design="dropdown" :label="internalLabel"></PButton>
            <svg :class="`${giveDesign}__icon`" xmlns="http://www.w3.org/2000/svg" width="9" height="4"
                viewBox="0 0 9 4">
                <path fill="currentColor" d="M4.5 4L0 0h9L4.5 4z" />
            </svg>
        </div>
        <ul v-if="dropDown" :class="`${giveDesign}__menu`">
            <li v-for="option in options" :value="option" @click="selectOption(description, option)">
                {{ typeof option === 'object' ? option.label : option }}
            </li>
        </ul>
    </div>
</template>