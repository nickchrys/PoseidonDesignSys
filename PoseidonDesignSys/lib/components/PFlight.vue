<script setup>

import { computed } from 'vue'

const props = defineProps({
    Label: String,
    flightDate: {
        type: Date,
        default: new Date(2025, 1, 1),
        required: true
    },
    origin: {
        type: String,
        default: 'QWE',
        required: true
    },
    destination: {
        type: String,
        default: 'ZXC',
        required: true
    },
    flightDepTime: {
        type: String,
        default: '00:00'
    },
    flightArrTime: {
        type: String,
        default: '00:00'
    },
    seatNumber: {
        type: String,
        default: '0A'
    },
    seatAvailable: {
        type: Number,
        default: 0
    },
    price: {
        type: Number,
        default: 0
    },
    flightType: {
        type: String,
        default: 'Nonstop'
    },
    flightClass: {
        type: String,
        default: 'Economy'
    },
    flightGate: {
        type: String,
        default: '00'
    },
    flightNumber: {
        type: String,
        default: '0000'
    },
    airline: {
        type: String,
        default: 'Airline'
    },
    logoURL: {
        type: String,
        default: 'default'
    },
    design: {
        type: String,
        default: 'default',
    }
});

const base = 'p-flight';
const giveDesign = computed(() => {
    const design = props.design !== 'default' ? `${base}--${props.design}` : base;
    return [design]
});

const getOrdIndicator = (day) => {
    if (day > 3 && day < 21) return 'th';
    switch (day % 10) {
        case 1: return 'st';
        case 2: return 'nd';
        case 3: return 'rd';
        default: return 'th';
    }
};

const formatTime = (time) => {
    const [hours, minutes] = time.split(':');
    const ampm = hours >= 12 ? 'PM' : 'AM';
    const formattedHours = hours % 12 || 12;
    return `${formattedHours}:${minutes} ${ampm}`;
};

const formatFlightDate = computed(() => {
    const options = { weekday: 'short', month: 'short', day: 'numeric' };
    const date = props.flightDate.toLocaleDateString('en-US', options);
    const day = props.flightDate.getDate();
    const month = props.flightDate.toLocaleDateString('en-US', { month: 'short' });
    const weekday = props.flightDate.toLocaleDateString('en-US', { weekday: 'short' });
    const ordIndicator = getOrdIndicator(day);
    return `${weekday}, ${month}. ${day}${ordIndicator}`;
});


</script>

<template>

    <div :class="giveDesign" @click="$emit('click')">

        <template v-if="giveDesign.includes('p-flight--itinerary')">
            <div class>
                <h3>{{ flightDate }}</h3>
                <div class="p-flight--itinerary__container">
                    <div class="p-flight--itinerary__time-place">
                        <div class="p-flight--itinerary__depart-container">
                            <div class="p-flight--itinerary__place">
                                <h3>Departure</h3>
                                <h2>{{ origin }}</h2>
                                <h3>City, State</h3>
                            </div>
                            <div class="p-flight--itinerary__time">
                                <h3>{{ formatTime(flightDepTime) }}</h3>
                            </div>
                        </div>
                        <div class="p-flight--itinerary__arrow">
                            <p>(Calculated time)</p>
                        </div>

                        <div class="p-flight--itinerary__arrive-container">
                            <div class="p-flight--itinerary__place">
                                <h3>Arrival</h3>
                                <h2>{{ destination }}</h2>
                                <h3>City, State</h3>
                            </div>
                            <div class="p-flight--itinerary__time">
                                <h3>{{ formatTime(flightArrTime) }}</h3>
                            </div>

                        </div>
                    </div>
                    <div class="p-flight--itinerary__info">
                        <h3>{{ flightNumber }}</h3>
                        <h3>{{ flightType }}</h3>
                        <h3>{{ flightClass }}</h3>
                        <h3>{{ seatNumber }}</h3>
                    </div>
                </div>
            </div>
        </template>

        <template v-else>
            <div class="p-flight__airline">
                <img class="p-flight__airline-logo" :src="logoURL" :alt="`${airline} Logo`" />
                <h5 class="p-flight__airline-text">{{ airline }}</h5>
            </div>
            <div class="p-flight__info">
                <div class="date-time-container">
                    <h5 class="p-flight__date">{{ formatFlightDate }}</h5>
                    <h5 class="p-flight__time">{{ formatTime(flightDepTime) }} - {{ formatTime(flightArrTime)
                        }}
                    </h5>
                </div>
                <div class="p-flight__details">
                    <h5>{{ origin }}-{{ destination }}</h5>
                    <h5>{{ flightType }}</h5>
                    <h5 v-if="giveDesign.includes('p-flight--block')">Seat {{ seatNumber }}</h5>
                    <h5 v-if="giveDesign.includes('p-flight--block')">Gate {{ flightGate }}</h5>
                    <h5 v-if="giveDesign.includes('p-flight--shop')">{{ seatAvailable }} Available Seats</h5>
                </div>
            </div>
            <div class="class-price-container">
                <h5 class="p-flight__class">{{ flightClass }}</h5>
                <h5 class="p-flight__price">${{ price }}</h5>
            </div>
        </template>
    </div>

</template>