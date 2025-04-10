<script setup>

import { ref, computed, watch } from 'vue'
import { format } from 'date-fns';

const emit = defineEmits(['update:description', 'update:modelValue', 'input', 'update:label'])

const props = defineProps({
    label: [String, Date],
    modelValue: [String, Date],
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
    },
    description: {
        type: String,
        default: 'description'
    }
})

const input = ref(props.modelValue || '')
const inputRef = ref(null)

const baseClass = 'p-textfield'
const giveDesign = computed(() => {
    return props.design !== 'default' ? `${baseClass}--${props.design}` : baseClass
})

// Compute the remaining characters dynamically
const remainingCharacters = computed(() => {
    const targetValue = props.type === 'textarea-edit' ? props.description : input.value
    return props.maxlength - (targetValue?.length || 0)
})

const showDatePicker = (event) => {
    event.target.type = 'date'
}

const hideDatePicker = (event) => {
    if (!input.value) {
        event.target.type = 'text'
    }
}

watch(() => props.modelValue, (newValue) => {
    if (newValue !== undefined) {
        input.value = newValue
    }
}, { immediate: true })

const updateValue = (value) => {
    if (props.type === 'date') {
        const dateValue = new Date(value)
        const formattedDate = format(dateValue, 'yyyy-MM-dd');
        input.value = formattedDate
        emit('update:modelValue', formattedDate)
        emit('update:label', formattedDate)
        emit('input', formattedDate)
    } else {
        input.value = value
        emit('update:modelValue', value)
        emit('input', value)
    }
}

defineExpose({
    focus: () => inputRef.value?.focus(),
    blur: () => inputRef.value?.blur()
})

</script>

<template>
    <template v-if="giveDesign.includes('p-textfield--textarea-edit')">
        <div class="p-textfield__textarea-container">
            <textarea :class="giveDesign" :placeholder="props.description || label" v-model="input"
                :maxlength="maxlength" @input="e => updateValue(e.target.value)"></textarea>
            <small class="p-textfield__char-counter">{{ remainingCharacters }} characters left</small>
        </div>
    </template>

    <template v-else-if="type === 'date'">
        <input ref="inputRef" :class="giveDesign" v-model="input" type="text" :placeholder="label"
            @focus="showDatePicker" @blur="hideDatePicker" @input="e => updateValue(e.target.value)" />
    </template>

    <template v-else-if="giveDesign.includes('p-textfield--textarea')">
        <div class="p-textfield__textarea-container">
            <textarea ref="inputRef" :class="giveDesign" v-model="input" :placeholder="label" :maxlength="maxlength"
                @input="e => updateValue(e.target.value)"></textarea>
            <small class="p-textfield__char-counter">{{ remainingCharacters }} characters left</small>
        </div>
    </template>

    <template v-else>
        <input ref="inputRef" autocomplete="on" :class="giveDesign" v-model="input" :type="type" :placeholder="label"
            @input="e => updateValue(e.target.value)" />
    </template>
</template>