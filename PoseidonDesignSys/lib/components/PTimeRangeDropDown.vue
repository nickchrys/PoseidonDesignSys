<script setup>
import { ref, computed } from 'vue'
import { PButton } from './index'
import VueSlider from 'vue-slider-component'
import 'vue-slider-component/theme/default.css'

const emit = defineEmits(['range-updated'])

const props = defineProps({
    dropDownLabel: {
        type: String,
        required: true
    },
    design: {
        type: String,
        default: '',
        validator: value => ['event', 'flight', 'login'].includes(value)
    },
    minTime: {
        type: Number,
        default: 0 // 0 = 00:00
    },
    maxTime: {
        type: Number,
        default: 1440 // 1440 = 24:00
    }
})

const base = 'p-dropdown'
const giveDesign = computed(() => {
    return props.design !== 'default' ? `${base}--${props.design}` : base
})

const dropDown = ref(false)
const dropDownLabel = computed(() => props.dropDownLabel)

const departureRange = ref([0, 1440]) // 00:00 to 24:00
const arrivalRange = ref([0, 1440]) // 00:00 to 24:00

const toggleDropDown = () => {
    dropDown.value = !dropDown.value
}

const formatTime = (minutes) => {
  const h = Math.floor(minutes / 60)
  const m = minutes % 60
  const period = h < 12 || h === 24 ? 'AM' : 'PM'
  const hour12 = h % 12 === 0 ? 12 : h % 12
  return `${hour12}:${String(m).padStart(2, '0')} ${period}`
}

const formatTime24hr = (minutes) => {
  const h = Math.floor(minutes / 60)
  const m = minutes % 60
  return `${String(h).padStart(2, '0')}:${String(m).padStart(2, '0')}`
}

const onDepRangeChange = (value, index) => {
    emit('range-updated', { type: 'departure', value: value.map(formatTime24hr) })
}

const onArrRangeChange = (value, index) => {
    emit('range-updated', { type: 'arrival', value: value.map(formatTime24hr) })
}

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

        <div v-if="dropDown" :class="`${giveDesign}__menu`" class="slider-dropdown">
            <div class="slider-group">
                <label>Departure Time: {{ formatTime(departureRange[0]) }} - {{ formatTime(departureRange[1]) }}</label>
                <VueSlider v-model="departureRange" :min="minTime" :max="maxTime" :interval="60" :tooltip="'hover'"
                    :tooltip-formatter="formatTime" :dot-size="16" :lazy="true" :marks="false"
                    :process-style="{ backgroundColor: '#FEB96E' }" @change="onDepRangeChange"/>
            </div>

            <div class="slider-group">
                <label>Arrival Time: {{ formatTime(arrivalRange[0]) }} - {{ formatTime(arrivalRange[1]) }}</label>
                <VueSlider v-model="arrivalRange" :min="minTime" :max="maxTime" :interval="60" :tooltip="'hover'"
                    :tooltip-formatter="formatTime" :dot-size="16" :lazy="true" :marks="false"
                    :process-style="{ backgroundColor: '#FEB96E' }" @change="onArrRangeChange"/>
            </div>
        </div>
    </div>
</template>

<style scoped>
.slider-dropdown {
    padding: 1rem;
    background: var(--pos-grey);
    border: 1px solid #ccc;
    border-radius: 8px;
    min-width: 300px;
}

.slider-group {
    margin-bottom: 1.5rem;
}

.slider-group label {
    display: block;
    font-weight: 600;
    margin-bottom: 0.5rem;
}
</style>
