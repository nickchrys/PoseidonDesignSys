<script setup>
import { computed, ref } from 'vue';
import PTextField from './PTextField.vue';
import { format, parseISO } from 'date-fns';
import VueDatePicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css'


const props = defineProps({
    id: { type: Number },
    organization: { type: Object, default: () => ({}) },
    eventName: { type: String, default: 'name' },
    startDate: { type: [Date, String], default: () => new Date(2025, 1, 1) },
    endDate: { type: [Date, String], default: () => new Date(2025, 1, 1) },
    destinationCode: { type: String, default: '---' },
    pictureLink: { type: String, default: 'pictureLink' },
    description: { type: String, default: 'description' },
    maxBudget: { type: Number, default: 0 },
    currentBudget: { type: Number, default: 0 },
    createdBy: { type: String, default: 'createdBy' },
    financeMan: { type: Object, default: () => ({}) },
    design: { type: String, default: 'default' },
    autoApprove: { type: Boolean, default: false },
    autoApproveThreshold: { type: Number, default: 0 },
    airline: { type: String, default: 'Airline' },
    eventDateRange: { type: Object, default: [] }
});

const emit = defineEmits(['eventClick', 'backClick', 'editClick', 'update']);

const giveDesign = computed(() => {
    return props.design !== 'default' ? `p-event--${props.design}` : 'p-event';
});

const parseDate = (date) => {
    if (!date) return null;
    return typeof date === 'string' ? parseISO(date) : date;
};

const formatDate = (date) => {
    const parsedDate = parseDate(date);
    if (!parsedDate) return '';
    return format(parsedDate, 'MMM. do');
};

const formatDateForBackend = (date) => {
    const parsedDate = parseDate(date);
    if (!parsedDate) return '';
    return format(parsedDate, "yyyy-MM-dd'T'HH:mm:ss");
};


const budgetColor = computed(() => {
    const threshold = props.maxBudget * 0.3;
    if (props.maxBudget > threshold) return 'var(--pos-green)';
    if (props.currentBudget >= 0) return 'var(--pos-yellow)';
    return 'var(--pos-red)';
});

const handleEventClick = () => {
    emit('eventClick', { ...props });
};

const handleEditClick = () => {
    emit('editClick', { ...props });
};

const handleBackClick = (e) => {
    e.stopPropagation();
    emit('backClick');
};

const editableName = ref(props.name);
const editableStartDate = ref(props.startDate);
const editableEndDate = ref(props.endDate);

const emitUpdate = (field, value) => {
    if (field === 'name') value = value;
    if (field === 'startDate') value = formatDateForBackend(value);
    if (field === 'endDate') value = formatDateForBackend(value);
    emit('update', { field, value });
};

</script>



<template>

    <template v-if="giveDesign.includes('p-event--org-block')">
        <div :class="giveDesign">
            <div class="org-container">
                <h1>{{ props.organization.name }}</h1>
            </div>
        </div>
    </template>

    <template v-else-if="giveDesign.includes('p-event--itinerary-header')">
        <div :class="giveDesign">
            <svg class="back-icon" @click="handleBackClick" xmlns="http://www.w3.org/2000/svg" width="16" height="32"
                viewBox="0 0 16 32">
                <path fill="currentColor" fill-rule="evenodd"
                    d="m3.343 12l7.071 7.071L9 20.485l-7.778-7.778a1 1 0 0 1 0-1.414L9 3.515l1.414 1.414z" />
            </svg>
            <div class="p-event--itinerary-header">
                <img :src="props.pictureLink" alt="Airline Logo" />
                <h1>{{ props.airline }}</h1>
                <h2>Flight Itinerary</h2>

            </div>
        </div>
    </template>

    <template v-else>
        <div :class="giveDesign" @click="handleEventClick(event)" :style="{
            background: `var(--gradient), url(${props.pictureLink})`,
            backgroundSize: 'cover',
            backgroundRepeat: 'no-repeat',
            backgroundPosition: 'center'
            }">
            <!-- Back Icon -->
            <svg v-if="giveDesign.includes('p-event--header') || giveDesign.includes('p-event--header-edit') || giveDesign.includes('p-event--small-header')"
                class="back-icon" @click="handleBackClick" xmlns="http://www.w3.org/2000/svg" width="16" height="32"
                viewBox="0 0 16 32">
                <path fill="currentColor" fill-rule="evenodd"
                    d="m3.343 12l7.071 7.071L9 20.485l-7.778-7.778a1 1 0 0 1 0-1.414L9 3.515l1.414 1.414z" />
            </svg>

            <!-- Edit Icon -->
            <svg v-if="giveDesign.includes('p-event--block-planner')" class="pencil-icon"
                xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" @click.stop="handleEditClick(event)">
                <g fill="none" fill-rule="evenodd">
                    <path
                        d="m12.593 23.258l-.011.002l-.071.035l-.02.004l-.014-.004l-.071-.035q-.016-.005-.024.005l-.004.01l-.017.428l.005.02l.01.013l.104.074l.015.004l.012-.004l.104-.074l.012-.016l.004-.017l-.017-.427q-.004-.016-.017-.018m.265-.113l-.013.002l-.185.093l-.01.01l-.003.011l.018.43l.005.012l.008.007l.201.093q.019.005.029-.008l.004-.014l-.034-.614q-.005-.018-.02-.022m-.715.002a.02.02 0 0 0-.027.006l-.006.014l-.034.614q.001.018.017.024l.015-.002l.201-.093l.01-.008l.004-.011l.017-.43l-.003-.012l-.01-.01z" />
                    <path fill="currentColor"
                        d="M20.131 3.16a3 3 0 0 0-4.242 0l-.707.708l4.95 4.95l.706-.707a3 3 0 0 0 0-4.243l-.707-.707Zm-1.414 7.072l-4.95-4.95l-9.09 9.091a1.5 1.5 0 0 0-.401.724l-1.029 4.455a1 1 0 0 0 1.2 1.2l4.456-1.028a1.5 1.5 0 0 0 .723-.401z"
                        stroke-width="1" stroke="#000" />
                </g>
            </svg>

            <!-- Event Details -->
            <h3 v-if="giveDesign.includes('p-event--header-edit')" class="p-event__dates">
                <!-- <PTextField v-model="editableStartDate" design="" id="Dates" type="date" -->
                <!-- @input="value => emitUpdate('startDate', value)" /> -->
                <VueDatePicker v-model="editableStartDate" id="Dates" :enable-time-picker="false"
                    :placeholder="'Start Date'" exactMatch="true"
                    :config="{ closeOnAutoApply: false, keepActionRow: true }" auto-apply hide-input-icon
                    @update:model-value="value => emitUpdate('startDate', value)"></VueDatePicker>
                <p>to</p>
                <VueDatePicker v-model="editableEndDate" id="Dates" :enable-time-picker="false"
                    :placeholder="'End Date'" exactMatch="true"
                    :config="{ closeOnAutoApply: false, keepActionRow: true }" auto-apply hide-input-icon
                    @update:model-value="value => emitUpdate('endDate', value)"></VueDatePicker>
                <!-- <PTextField v-model="editableEndDate" design="" id="Dates" type="date" -->
                <!-- @input="value => emitUpdate('endDate', value)" /> -->
            </h3>
            <h3 v-else-if="!giveDesign.includes('p-event--small-header') && !giveDesign.includes('p-event--desktop-header')" class="p-event__dates">{{
                formatDate(startDate) }} - {{ formatDate(endDate) }}</h3>
            <h3 v-if="giveDesign.includes('p-event--block-finance')" class="p-event__budget"
                :style="{ color: budgetColor }">
                Budget<br />{{ Math.round((currentBudget / maxBudget) * 100) }}%
            </h3>
            <div class="p-event__details">
                <h2 v-if="!giveDesign.includes('p-event--small-header')">{{ eventName }}</h2>
                <h4 v-if="!giveDesign.includes('p-event--header-edit') && !giveDesign.includes('p-event--desktop-header')">{{ props.organization.name }}</h4>
                <h4 v-if="giveDesign.includes('p-event--desktop-header')">Hosted by {{ props.organization.name }}</h4>
                <h4 v-else-if="giveDesign.includes('p-event--header-edit')">
                    <PTextField v-model="editableName" design="textarea-edit" id="Title" :description="eventName"
                        @update:modelValue="value => emitUpdate('name', value)" />
                </h4>
            </div>
        </div>
    </template>

</template>