<script setup>

import { computed } from 'vue';
import { PButton, PProfilePic } from './index'

const props = defineProps({
    email: {
        type: String,
        default: 'email@org.com'
    },
    name: {
        type: String,
        default: 'FName LName'
    },
    jobTitle: {
        type: String,
        default: 'Finance Person'
    },
    phoneNum: {
        type: String,
        default: '123-456-7890'
    },
    profileImage: {
        type: String,
        default: 'src/assets/defaultPP.PNG'
    },
    design: {
        type: String,
        default: 'default',
    }
})

const giveDesign = computed(() => {
    return props.design !== 'default' ? `p-finance--${props.design}` : 'p-finance';
});

</script>

<template>

    <template v-if="giveDesign.includes('p-finance--invite')">
            <div :class='giveDesign'> 
                <div class="pic-container">
                    <PProfilePic design='med' :profileImage="profileImage" />
                </div>
                <div class="p-finance__info">
                    <h1>{{ name }}</h1>
                </div>
            </div>
    </template>

    <template v-else>

        <div :class='giveDesign'>
            <div class="finance-button-pic-container">
                <PProfilePic design='finance' :profileImage="profileImage" />
                <PButton design='gradient-small' label='Talk with me!' />
            </div>
            <div class="p-finance__info">
                <h1>{{ name }}</h1>
                <h2>{{ jobTitle }}</h2>

                <div class="p-finance__mail">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
                        <defs>
                            <linearGradient id="icon-gradient" x1="0.5" y1="0" x2="0.5" y2="1">
                                <stop offset="0%" stop-color="#4c365d" />
                                <stop offset="25%" stop-color="#523f70" />
                                <stop offset="50%" stop-color="#524a88" />
                                <stop offset="100%" stop-color="#566688" />
                            </linearGradient>
                        </defs>
                        <path fill="url(#icon-gradient)"
                            d="m2.068 5.482l8.875 8.876a1.5 1.5 0 0 0 2.008.103l.114-.103l8.869-8.87q.043.165.058.337L22 6v12a2 2 0 0 1-1.85 1.995L20 20H4a2 2 0 0 1-1.995-1.85L2 18V6q0-.18.03-.35zM20 4q.182 0 .355.031l.17.039l-8.52 8.52l-8.523-8.522q.166-.045.34-.06L4 4z" />
                    </svg>
                    <h3>{{ email }}</h3>
                </div>

                <div class="p-finance__phone">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
                        <path fill="url(#icon-gradient)"
                            d="m16.556 12.906l-.455.453s-1.083 1.076-4.038-1.862s-1.872-4.014-1.872-4.014l.286-.286c.707-.702.774-1.83.157-2.654L9.374 2.86C8.61 1.84 7.135 1.705 6.26 2.575l-1.57 1.56c-.433.432-.723.99-.688 1.61c.09 1.587.808 5 4.812 8.982c4.247 4.222 8.232 4.39 9.861 4.238c.516-.048.964-.31 1.325-.67l1.42-1.412c.96-.953.69-2.588-.538-3.255l-1.91-1.039c-.806-.437-1.787-.309-2.417.317" />
                    </svg>
                    <h3>{{ phoneNum }}</h3>
                </div>
            </div>
        </div>

    </template>

</template>