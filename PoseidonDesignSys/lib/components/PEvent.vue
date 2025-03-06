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
        type: [Date, String],
        default: new Date(2025, 1, 1)
    },
    endDate: {
        type: [Date, String],
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

const parseDate = (date) => {
    return date ? (typeof date === 'string' ? new Date(date) : date) : null;
}

const formattedStartDate = computed(() => {
    const date = parseDate(props.startDate);
    if (!date) return '';
    const day = date.getDate();
    const month = date.toLocaleDateString('en-US', { month: 'short' });
    return `${month}. ${day}${getOrdIndicator(day)}`;
});

const formattedEndDate = computed(() => {
    const date = parseDate(props.endDate);
    if (!date) return '';
    const day = date.getDate();
    const month = date.toLocaleDateString('en-US', { month: 'short' });
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
    } else if (props.currentBudget >= 0 && props.currentBudget <= budgetThreshold) {
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
                    <circle cx="2.5" cy="2.5" r="2.5" stroke="black" stroke-width=".7" />
                    <circle cx="14.5" cy="2.5" r="2.5" stroke="black" stroke-width=".7" />
                    <circle cx="26.5" cy="2.5" r="2.5" stroke="black" stroke-width=".7" />
                </g>
            </svg>
            <svg class="pencil-icon" v-if="giveDesign.includes('p-event--block-planner')"
                xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24">
                <g fill="none" fill-rule="evenodd">
                    <path
                        d="m12.593 23.258l-.011.002l-.071.035l-.02.004l-.014-.004l-.071-.035q-.016-.005-.024.005l-.004.01l-.017.428l.005.02l.01.013l.104.074l.015.004l.012-.004l.104-.074l.012-.016l.004-.017l-.017-.427q-.004-.016-.017-.018m.265-.113l-.013.002l-.185.093l-.01.01l-.003.011l.018.43l.005.012l.008.007l.201.093q.019.005.029-.008l.004-.014l-.034-.614q-.005-.018-.02-.022m-.715.002a.02.02 0 0 0-.027.006l-.006.014l-.034.614q.001.018.017.024l.015-.002l.201-.093l.01-.008l.004-.011l.017-.43l-.003-.012l-.01-.01z" />
                    <path fill="currentColor"
                        d="M20.131 3.16a3 3 0 0 0-4.242 0l-.707.708l4.95 4.95l.706-.707a3 3 0 0 0 0-4.243l-.707-.707Zm-1.414 7.072l-4.95-4.95l-9.09 9.091a1.5 1.5 0 0 0-.401.724l-1.029 4.455a1 1 0 0 0 1.2 1.2l4.456-1.028a1.5 1.5 0 0 0 .723-.401z"
                        stroke-width="1" stroke="#000" />
                </g>
            </svg>
            <h3 class="p-event__dates">{{ formattedStartDate }} - {{ formattedEndDate }}</h3>
            <h3 class="p-event__budget" v-if="giveDesign.includes('p-event--block-finance')"
                :style="{ color: budgetColor }">Budget <br>${{ currentBudget }}/${{ maxBudget }}</h3>
            <div class="p-event__details">
                <h2>{{ organization }}</h2>
                <div>
                    <h4>{{ name }}</h4>
                </div>
            </div>
        </div>
    </template>

</template>