<script setup>
import { computed } from 'vue'

const props = defineProps({
    Label: String,
    flightDate: {
        type: Date,
        default: new Date(2025, 1, 1)
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
    flightRoute: {
        type: String,
        default: 'QWE-ASD'
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
    airline: {
        type: String,
        default: 'Airline'
    },
    airlineLogo: {
        type: String,
        default: ''
    },
    class: String
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

    <div :class="class">
        <div class="flight-block">
            <div class="airline">
                <img class="airline-logo" :src="`src/assets/img/AirlineLogos/${ props.airline }.png `" alt="Airline Logo" />
                <h5 class="airline-text">{{ props.airline }}</h5>
            </div>
            <div class="flight-info-mid-content">
                <div class="date-time-container">
                    <h5 class="flight-date">{{ formatFlightDate }}</h5>
                    <h5 class="flight-time">{{ formatTime(props.flightDepTime) }} - {{ formatTime(props.flightArrTime)
                        }}

                    </h5>
                </div>
                <div class="flight-details">
                    <h5>{{ props.flightRoute }}</h5>
                    <h5>Seat {{ props.seatNumber }}</h5>
                    <h5>{{ props.flightType }}</h5>
                    <h5>Gate {{ props.flightGate }}</h5>
                </div>

            </div>
            <div class="class-price-container">
                <h5 class="flight-class">{{ props.flightClass }}</h5>
                <h5 class="flight-price">${{ props.price }}</h5>
            </div>
        </div>
    </div>

</template>