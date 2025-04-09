<script setup>

import { computed } from 'vue'

const props = defineProps({
    Label: String,
    flightDate: {
        type: Date,
        default: new Date(2025, 1, 1),

    },
    flightID: {
        type: String,
        default: '',
    },
    passangerName: {
        type: String,
        default: 'Name',
        required: false
    },
    origin: {
        type: String,
        default: 'AAA',
    },
    originCity: {
        type: String,
        default: 'City',
    },
    destination: {
        type: String,
        default: 'BBB',

    },
    destinationCity: {
        type: String,
        default: 'City',
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
        default: 'Type'
    },
    flightClass: {
        type: String,
        default: 'Class'
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
    },
    itinerary: {
        type: Object,
        default: {},
    },
    flightDuration: {
        type: String,
        default: '(Calculated time)'
    },
    layoverDuration: {
        type: String,
        default: '00 hr 00 m'
    },
    currentIndex: {
        type: Number,
        required: false,
        default: 1
    },
    totalFlights: {
        type: Number,
        required: false,
        default: 1
    },
    depOrArr: {
        type: String,
        default: ''
    },
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

const formatDuration = (duration) => {
    const hoursMatch = duration.match(/(\d+)H/);
    const minutesMatch = duration.match(/(\d+)M/);

    const hours = hoursMatch ? `${hoursMatch[1]}h` : '';
    const minutes = minutesMatch ? `${minutesMatch[1]}m` : '';

    return [hours, minutes].filter(Boolean).join(' ');
};



const formatLongDuration = (duration) => {
    const hoursMatch = duration.match(/(\d+)H/);
    const minutesMatch = duration.match(/(\d+)M/);

    const hours = hoursMatch ? `${hoursMatch[1]} hour` : '';
    const minutes = minutesMatch ? `${minutesMatch[1]} minutes` : '';

    return [hours, minutes].filter(Boolean).join(' & ');
};

const formatFlightDate = computed(() => {
    // console.log("SHORT",props.flightDate)
    const options = { weekday: 'short', month: 'short', day: 'numeric' };
    const date = props.flightDate.toLocaleDateString('en-US', options);
    const day = props.flightDate.getDate();
    const month = props.flightDate.toLocaleDateString('en-US', { month: 'short' });
    const weekday = props.flightDate.toLocaleDateString('en-US', { weekday: 'short' });
    const ordIndicator = getOrdIndicator(day);
    return `${weekday}, ${month}. ${day}${ordIndicator}`;

});

const formatLongFlightDate = computed(() => {
    // console.log("LONG",props.flightDate)
    const options = { weekday: 'long', month: 'long', day: 'numeric' };
    const date = props.flightDate.toLocaleDateString('en-US', options);
    const day = props.flightDate.getDate();
    const month = props.flightDate.toLocaleDateString('en-US', { month: 'long' });
    const weekday = props.flightDate.toLocaleDateString('en-US', { weekday: 'long' });
    const year = props.flightDate.getFullYear();
    const ordIndicator = getOrdIndicator(day);
    return `${weekday}, ${month} ${day}${ordIndicator}, ${year}`;
});


const eventData = localStorage.getItem('currentEvent')
// Set the color of the price based on the autoapprove threshold
// If the eventData is not null, parse it and check the autoapprove property
// If autoapprove is true, check the price against the threshold. If false, return null
// If the price is greater than 90% of the threshold, set color to red
// If the price is less than 50% of the threshold, set color to green
// Otherwise, set color to orange
const priceColor = computed(() => {
    if (eventData) {
        const parsedEventData = JSON.parse(eventData);
        if (parsedEventData.autoapprove) {
            const threshold = parsedEventData.autoapprove_threshold;
            const ratio = props.price / threshold;
            if (ratio > .9) return 'var(--pos-red)';       // More than 90% of threshold → Expensive
            if (ratio < 0.5) return 'var(--pos-green)';     // Below half threshold → Cheap
            return 'var(--pos-yellow)';
        } else {
            return null;
        }
    }
});


</script>



<template>

    <div :class="giveDesign" @click="$emit('click')">

        <template v-if="giveDesign.includes('p-flight--itinerary')">
            <div class>
                <h3 class="p-flight--itinerary__date">{{ formatLongFlightDate }}</h3>
                <div class="p-flight--itinerary__container">
                    <div class="p-flight--itinerary__time-place">
                        <div class="p-flight--itinerary__depart-container">
                            <div class="p-flight--itinerary__place">
                                <h3>Departure</h3>
                                <h2>{{ origin }}</h2>
                                <h4>City, State</h4>
                            </div>
                            <div class="p-flight--itinerary__time">
                                <h3>{{ formatTime(flightDepTime) }}</h3>
                            </div>
                        </div>
                        <div class="p-flight--itinerary__arrow">
                            <svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="0 0 24 24">
                                <path fill="#000"
                                    d="M12 21.9q-.2 0-.375-.075T11.3 21.6l-5.6-5.575q-.275-.275-.275-.7T5.7 14.6q.3-.3.713-.3t.712.3L11 18.5v-6.175q0-.425.288-.712t.712-.288t.713.288t.287.712V18.5l3.9-3.9q.275-.275.688-.275t.712.3q.275.275.275.7t-.275.7L12.7 21.6q-.15.15-.325.225T12 21.9m0-12.575q-.425 0-.712-.287T11 8.325v-1q0-.425.288-.712T12 6.325t.713.288t.287.712v1q0 .425-.288.713T12 9.325m0-5q-.425 0-.712-.287T11 3.325t.288-.712t.712-.288t.713.288t.287.712t-.288.713t-.712.287" />
                            </svg>
                            <p>{{ formatDuration(flightDuration) }}</p>
                        </div>

                        <div class="p-flight--itinerary__arrive-container">
                            <div class="p-flight--itinerary__place">
                                <h3>Arrival</h3>
                                <h2>{{ destination }}</h2>
                                <h4>City, State</h4>
                            </div>
                            <div class="p-flight--itinerary__time">
                                <h3>{{ formatTime(flightArrTime) }}</h3>
                            </div>

                        </div>
                    </div>
                    <div class="p-flight--itinerary__info">

                        <div class="p-flight--itinerary__info-container">
                            <h4>Flight</h4>
                            <h3>{{ flightNumber }}</h3>
                        </div>

                        <div class="p-flight--itinerary__info-container">
                            <h4>Class</h4>
                            <h3>{{ flightClass }}</h3>
                        </div>

                        <div class="p-flight--itinerary__info-container">
                            <h4>Seat</h4>
                            <h3>{{ seatNumber }}</h3>
                        </div>

                        <!-- <div class="p-flight--itinerary__info-container">
                            <h4>Type</h4>
                            <h3>{{ flightType }}</h3>
                        </div> -->

                    </div>
                </div>
            </div>
        </template>

        <template v-else-if="giveDesign.includes('p-flight--desktop-block')">
            <div :class="giveDesign">
                <div class="p-flight__airline">
                    <img class="p-flight__airline-logo" :src="logoURL" :alt="`${airline} Logo`" />
                    <div class="p-flight__airline-time">
                        <h3 class="p-flight__time">{{ formatTime(flightDepTime) }} - {{ formatTime(flightArrTime) }}
                        </h3>
                        <h5 class="p-flight__airline-text">{{ airline }}</h5>
                    </div>
                </div>
                <div class="p-flight__duration-codes">
                    <h3>{{ formatDuration(flightDuration) }}</h3>
                    <h5 class="p-flight__codes">{{ origin }}-{{ destination }}</h5>
                </div>

                <h3 class="p-flight__type">{{ flightType }}</h3>

                <div class="p-flight__flight-number-seats">
                    <h3 class="p-flight__flight-number">Flight {{ flightNumber }}</h3>
                    <h5 class="p-flight__seats">Gate {{ flightGate }}</h5>
                </div>

                <div class="p-flight__price-container">
                    <h5 class="p-flight__class">{{ flightClass }}</h5>
                    <h5 class="p-flight__price" :style="{ color: priceColor }">{{
                        Math.round(price).toLocaleString('en-US',
                            { style: 'currency', currency: 'USD', minimumFractionDigits: 0, maximumFractionDigits: 0 }) }}
                    </h5>
                </div>

            </div>

        </template>

        <template v-else-if="giveDesign.includes('p-flight--desktop-itinerary')">
            <div class="itnry-header">
                <h3>Flight {{ flightNumber }} - <b>{{ currentIndex }} of {{ totalFlights }}</b> </h3>
                <h3 class="itnry-class">Class: {{ flightClass }}</h3>
            </div>
            <div class="itnry-desktop-container">
                <div class="p-flight__airline">
                    <img class="p-flight__airline-logo" :src="logoURL" :alt="`${airline} Logo`" />
                    <p class="p-flight__airline-text">{{ airline }}</p>
                </div>

                <div class="itnry-arrival-container">
                    <h2 class="itnry-arrival__time">{{ formatTime(flightDepTime) }}</h2>
                    <h3 class="itnry-arrival__place">{{ origin }}</h3>
                    <h4 class="itnry-arrival__city">{{ originCity }}</h4>
                </div>
                <div class="itnry-plane-section">
                    <h5 class="itnry-date">{{ formatLongFlightDate }}</h5>
                    <svg xmlns="http://www.w3.org/2000/svg" width="3.7em" height="3.7em" viewBox="0 0 1200 1200">
                        <path fill="#4c365d"
                            d="M321 1164h120l269.28-480.06H1020s180 0 180-83.94c0-84-180-84-180-84H710.28L441 36H321l149.28 480H255.06L120 395.94H0l96.06 204L0 804h120l135.06-120.06h215.22z" />
                    </svg>
                    <h5 class="flight-duration">{{ formatLongDuration(flightDuration) }}</h5>
                </div>
                <div class="itnry-departure-container">
                    <h2 class="itnry-departure__time">{{ formatTime(flightArrTime) }}</h2>
                    <h3 class="itnry-departure__place">{{ destination }}</h3>
                    <h4 class="itnry-departure__city">{{ destinationCity }}</h4>
                </div>
            </div>
        </template>

        <template v-else-if="giveDesign.includes('p-flight--finance')">
            <div class="p-flight__airline">
                <img class="p-flight__airline-logo" :src="logoURL" :alt="`${airline} Logo`" />
                <h5 class="p-flight__airline-text">{{ airline }}</h5>
            </div>
            <div class="p-flight__info">
                <h5 class="p-flight--finance__name">{{ passangerName }}'s Ticket</h5>
                <h5 class="p-flight__class">{{ flightClass }}</h5>
                <h5 class="p-flight__date">{{ formatFlightDate }}</h5>
                <h5 class="p-flight__time">{{ formatTime(flightDepTime) }} - {{ formatTime(flightArrTime) }}</h5>
            </div>
            <div class="class-price-container">
                <h5 class="p-flight__price">{{ price.toLocaleString('en-US', { style: 'currency', currency: 'USD' }) }}
                </h5>
            </div>
        </template>

        <template v-else-if="giveDesign.includes('p-flight--layover')">
            <div class="p-flight--layover__container">
                <h4>Layover: {{ layoverDuration }}</h4>
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
                <h5 class="p-flight__price" :style="{ color: priceColor }">{{ Math.round(price).toLocaleString('en-US',
                    { style: 'currency', currency: 'USD', minimumFractionDigits: 0, maximumFractionDigits: 0 }) }}</h5>
            </div>
        </template>
    </div>

</template>