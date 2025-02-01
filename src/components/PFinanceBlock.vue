<script setup>
import PButton from './PButton.vue';
import PProfilePic from './PProfilePic.vue';


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
    class: String
});

</script>

<template>

    <div :class='props.class'>
        <div class="finance-button-pic">
            <PProfilePic class='profile-pic-big' />
            <PButton msg='Talk with me!' class='gradButtonSmall' />
        </div>
        <div class="finance-info">
            <h1>{{ props.name }}</h1>
            <h2>{{ props.jobTitle }}</h2>

            <div class="mail-icon">
                <svg class="finance-icon" xmlns="http://www.w3.org/2000/svg" width="24" height="24"
                    viewBox="0 0 24 24"></svg>
                <h3>{{ props.email }}</h3>
            </div>

            <div class="phone-icon">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
                    <path fill="currentColor"
                        d="m16.556 12.906l-.455.453s-1.083 1.076-4.038-1.862s-1.872-4.014-1.872-4.014l.286-.286c.707-.702.774-1.83.157-2.654L9.374 2.86C8.61 1.84 7.135 1.705 6.26 2.575l-1.57 1.56c-.433.432-.723.99-.688 1.61c.09 1.587.808 5 4.812 8.982c4.247 4.222 8.232 4.39 9.861 4.238c.516-.048.964-.31 1.325-.67l1.42-1.412c.96-.953.69-2.588-.538-3.255l-1.91-1.039c-.806-.437-1.787-.309-2.417.317" />
                </svg>
                <h3>{{ props.phoneNum }}</h3>
            </div>

        </div>
    </div>

</template>