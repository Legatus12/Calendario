<template>
    <div class="day-container">
        <DragEvent :list=list :day=generateID(date) :selectDay=selectDay @SendEvent="sendEvent"/>
    </div>
</template>

<script setup>

import DragEvent from './EventManagement/DragEvent.vue';

const emits = defineEmits(["SendEvent"])

const props = defineProps({
    date: Object,
    // en proceso de borrar probar si funcionar sin dayNames: Array,
    list: Array,
    selectDay: Boolean,
});

const sendEvent = event => emits("SendEvent",event);

const generateID = day => String(day.year()) + '-' + String(day.month() + 1).padStart(2, '0') + '-' + String(day.date()).padStart(2, '0');

</script>

<style scoped>

.day-container{
    @apply flex flex-col p-12
}

</style>