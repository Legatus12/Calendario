<template>
    <div class="main-container">
        <div class="header-container">
            <select v-model="selectedComponent" class="select">
                <option v-for="option in options" v-bind:value="option.value">
                    {{ option.text }}
                </option>
            </select>

            <div class="flex justify-center items-center p-4 gap-8">
                <button class="date-button" @click="getPreviousDay">&lt;</button>
                <div>{{ selectedDate.Day }}</div>
                <button class="date-button" @click="getNextDay">&gt;</button>
            </div>
        </div>

        <div class="body-container">
            <component v-bind:is="selectedComponent" class="component" />
        </div>
        <AddEvent />
    </div>



</template>
  
<script setup>

import { reactive, shallowRef } from "vue";

import Day from './Day.vue';
import Week from './Week.vue';
import Month from './Month.vue';
import AddEvent from "./AddEvent/AddEvent.vue";

const selectedComponent = shallowRef(Day);
const options = [
    { text: 'Day', value: Day },
    { text: 'Week', value: Week },
    { text: 'Month', value: Month }
]

const currentDate = (new Date());

const selectedDate = reactive({ Date: currentDate, Day: currentDate.getDate() });

const getPreviousDay = () => {

    selectedDate.Date.setDate(selectedDate.Date.getDate() - 1);
    selectedDate.Day = selectedDate.Date.getDate();
    console.log(selectedDate.Date)
}

const getNextDay = () => {
    selectedDate.Date.setDate(selectedDate.Date.getDate() + 1);
    selectedDate.Day = selectedDate.Date.getDate();
    console.log(selectedDate.Date)
}


</script>

<style scoped>
.main-container {
    @apply flex flex-col w-full h-full border-solid border-2 border-black rounded-2xl
}

.header-container {
    @apply flex justify-around items-center
}

.select {
    @apply border-solid border-2 border-black p-4 rounded-full cursor-pointer
}

.date-button {
    @apply w-12 h-12 border-solid border-2 border-black rounded-full
}

.body-container {
    @apply h-full w-full p-4
}

.component {
    @aplly border-solid border-2 border-black rounded-full
}
</style>