<template>
    <div class="day-container">
        <DragEvent :list=list :day=generateID(date) />
    </div>
</template>

<script setup>

import DragEvent from './AddEvent/DragEvent.vue';

//

const props = defineProps({
    date: Object,
    dayNames: Array,
    list: Array,
});

const generateID = day => String(day.year()) + '-' + String(day.month() + 1).padStart(2, '0') + '-' + String(day.date()).padStart(2, '0');

</script>

<style scoped>

.day-container{
    @apply flex flex-col p-12
}

</style>