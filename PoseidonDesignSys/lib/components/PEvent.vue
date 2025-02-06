<script setup>

import { computed } from 'vue'

const props = defineProps({
    label: String,
    organization: {
        type: String,
        default: 'organization'
    },
    event: {
        type: String,
        default: 'event'
    },
    startDate: {
        type: Date,
        default: new Date(2025, 1, 1)
    },
    endDate: {
        type: Date,
        default: new Date(2025, 1, 1)
    },
    bgImage: {
        type: String,
        default: 'bgImage'
    },
    design: {
        type: String,
        default: 'default',
    }
});

const base = 'p-event';
const giveDesign = computed(() => {
    const design = props.design !== 'default' ? `${base}--${props.design}` : base;
    return [design]
})

const getOrdIndicator = (day) => {
    if (day > 3 && day < 21) return 'th';
    switch (day % 10) {
        case 1: return 'st';
        case 2: return 'nd';
        case 3: return 'rd';
        default: return 'th';
    }
}

const formattedStartDate = computed(() => {
    const day = props.startDate.getDate();
    const month = props.startDate.toLocaleDateString('en-US', { month: 'short' });
    return `${month}. ${day}${getOrdIndicator(day)}`;
});

const formattedEndDate = computed(() => {
    const day = props.endDate.getDate();
    const month = props.endDate.toLocaleDateString('en-US', { month: 'short' });
    return `${month}. ${day}${getOrdIndicator(day)}`;
});

const emit = defineEmits(['eventClick', 'backClick'])

const handleEventClick = () => {
    emit('eventClick', {
        organization: props.organization,
        event: props.event,
        startDate: props.startDate,
        endDate: props.endDate,
        bgImage: props.bgImage
    })
}

const handleBackClick = (e) => {
    e.stopPropagation() // Prevent event bubbling
    emit('backClick')
}

</script>

<template>
    <div :class='giveDesign' @click="handleEventClick" :style="{
        background: `var(--gradient), url(${props.bgImage})`,
        backgroundSize: 'cover',
        backgroundRepeat: 'no-repeat',
        backgroundPosition: 'center'
    }">
        <svg class="back-icon" v-if="giveDesign.includes('p-event--header')"  @click="handleBackClick" xmlns="http://www.w3.org/2000/svg"
            width="16" height="32" viewBox="0 0 16 32">
            <path fill="currentColor" fill-rule="evenodd"
                d="m3.343 12l7.071 7.071L9 20.485l-7.778-7.778a1 1 0 0 1 0-1.414L9 3.515l1.414 1.414z" />
        </svg>
        <svg class="more-icon" xmlns="http://www.w3.org/2000/svg" width="29" height="5" viewBox="-0.5 -0.5 30 6">
            <g fill="#D9D9D9">
                <circle cx="2.5" cy="2.5" r="2.5" stroke="black" stroke-width="0.3" />
                <circle cx="14.5" cy="2.5" r="2.5" stroke="black" stroke-width="0.3" />
                <circle cx="26.5" cy="2.5" r="2.5" stroke="black" stroke-width="0.3" />
            </g>
        </svg>
        <h3 class="p-event__dates">{{ formattedStartDate }} - {{ formattedEndDate }}</h3>
        <div class="p-event__details">
            <h2>{{ organization }}</h2>
            <h4>{{ event }}</h4>
        </div>
    </div>
</template>