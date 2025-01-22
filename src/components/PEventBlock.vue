<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
    label: String,
    organization: {
        organization: String,
        default: 'organization'
    },
    event: {
        event: String,
        default: 'event'
    },
    startDate: {
        startDate: new Date(),
        default: new Date(2025, 1, 1)
    },
    endDate: {
        startDate: new Date(),
        default: new Date(2025, 1, 1)
    },
    bgImage: {
        bgImage: String,
        default: 'bgImage'
    },
    class: String
});

const formattedStartDate = computed(() => { 
    return props.startDate.toLocaleDateString('en-US', {
    month: 'short',
    day: 'numeric',
    });
});

const formattedEndDate = computed(() => { 
    return props.endDate.toLocaleDateString('en-US', {
    month: 'short',
    day: 'numeric',
    });
});
//style="{ backgroundImage: `url(${bgImage})` }"
const input = ref('')
</script>

<template>
    <div :class='class'>
        <div class="eventBlockImage">
            <div class="eventBlockContent">
                <h3>{{ formattedStartDate }} - {{ formattedEndDate }}</h3>
                <div class="eventBlockDetails">
                    <h2>{{ organization }}</h2>
                    <h4>{{ event }}</h4>
                </div>
            </div>
        </div>
    </div>
</template>