<script setup>

import { computed } from 'vue'

const props = defineProps({
    label: String,
    organization: {
        type: String,
        default: 'organization'
    },
    name: {
        type: String,
        default: 'name'
    },
    startDate: {
        type: Date,
        default: new Date(2025, 1, 1)
    },
    endDate: {
        type: Date,
        default: new Date(2025, 1, 1)
    },
    pictureLink: {
        type: String,
        default: 'pictureLink'
    },
    description: {
        type: String,
        default: 'description'
    },
    maxBudget: {
        type: Number,
        default: 0
    },
    currentBudget: {
        type: Number,
        default: 0
    },
    createdBy: {
        type: String,
        default: 'createdBy'
    },
    financeMan: {
        type: String,
        default: 'financeMan'
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
        name: props.name,
        startDate: props.startDate,
        endDate: props.endDate,
        pictureLink: props.pictureLink,
        description: props.description,
        maxBudget: props.maxBudget,
        currentBudget: props.currentBudget,
        createdBy: props.createdBy,
        financeMan: props.financeMan,
        inviteLink: props.inviteLink
    })
}

const handleBackClick = (e) => {
    e.stopPropagation() // Prevent event bubbling
    emit('backClick')
}

const budgetColor = computed(() => {
    const budgetThreshold = props.maxBudget * 0.3;
    if (props.currentBudget > budgetThreshold) {
        return 'var(--pos-green)';
    } else if (props.currentBudget >= 0) {
        return 'var(--pos-yellow)';
    } else {
        return 'var(--pos-red)';
    }
});

</script>
<template>

    <template v-if="giveDesign.includes('p-event--itinerary-header')">
        <div :class="giveDesign">
            <svg class="back-icon" @click="handleBackClick" xmlns="http://www.w3.org/2000/svg" width="16" height="32"
                viewBox="0 0 16 32">
                <path fill="currentColor" fill-rule="evenodd"
                    d="m3.343 12l7.071 7.071L9 20.485l-7.778-7.778a1 1 0 0 1 0-1.414L9 3.515l1.414 1.414z" />
            </svg>
            <div class="p-event--itinerary-header">
                <img :src="props.pictureLink" alt="Airline Logo" />
                <h1>{{ props.organization }}</h1>
                <h2>Flight itinerary</h2>

            </div>
        </div>
    </template>


    <template v-else>
        <div :class='giveDesign' @click="handleEventClick" :style="{
            background: `var(--gradient), url(${props.pictureLink})`,
            backgroundSize: 'cover',
            backgroundRepeat: 'no-repeat',
            backgroundPosition: 'center'
        }">
            <svg class="back-icon" v-if="giveDesign.includes('p-event--header')" @click="handleBackClick"
                xmlns="http://www.w3.org/2000/svg" width="16" height="32" viewBox="0 0 16 32">
                <path fill="currentColor" fill-rule="evenodd"
                    d="m3.343 12l7.071 7.071L9 20.485l-7.778-7.778a1 1 0 0 1 0-1.414L9 3.515l1.414 1.414z" />
            </svg>
            <svg class="more-icon" xmlns="http://www.w3.org/2000/svg" width="29" height="5" viewBox="-0.5 -0.5 30 6">
                <g fill="#D9D9D9">
                    <circle cx="2.5" cy="2.5" r="2.5" stroke="black" stroke-width="0.5" />
                    <circle cx="14.5" cy="2.5" r="2.5" stroke="black" stroke-width="0.5" />
                    <circle cx="26.5" cy="2.5" r="2.5" stroke="black" stroke-width="0.5" />
                </g>
            </svg>
            <h3 class="p-event__dates">{{ formattedStartDate }} - {{ formattedEndDate }}</h3>
            <h3 class="p-event__budget" v-if="giveDesign.includes('p-event--block-finance')"
                :style="{ color: budgetColor }">Budget <br>${{ currentBudget }}</h3>
            <div class="p-event__details">
                <h2>{{ organization }}</h2>
                <h4>{{ name }}</h4>
            </div>
        </div>
    </template>

</template>