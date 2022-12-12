<template>
    <div class="main-container">
        <div class="header-container">
            <select v-model="selectedComponent" class="select">
                <option v-for="option in options" v-bind:value="option.value">
                    {{ option.text }}
                </option>
            </select>

            <div class="button-container">
                <button class="date-button" @click="getPreviousDay">&lt;</button>

                <div>{{ selectedDate.day }} / {{ selectedDate.month + 1 }} / {{ selectedDate.year }}</div>

                <button class="date-button" @click="getNextDay">&gt;</button>
            </div>
        </div>
        
        <div class="body-container">
            <component v-bind:is="selectedComponent" />
        </div>
    </div>

    <AddEvent />

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


const selectedDate = reactive({ Date: currentDate, day: currentDate.getDate(), month: currentDate.getMonth(), year: currentDate.getFullYear() });


const getPreviousDay = () => {

    selectedDate.Date.setDate(selectedDate.Date.getDate() - 1);
    selectedDate.day = selectedDate.Date.getDate();
    selectedDate.month = currentDate.getMonth();
    selectedDate.year = currentDate.getFullYear();
    console.log(selectedDate.Date)
}

const getNextDay = () => {
    selectedDate.Date.setDate(selectedDate.Date.getDate() + 1);
    selectedDate.day = selectedDate.Date.getDate();
    selectedDate.month = currentDate.getMonth();
    selectedDate.year = currentDate.getFullYear();
    console.log(selectedDate.Date)
}


</script>

<style scoped>
.main-container {
    @apply flex flex-col w-full h-full border-solid border-2 border-black rounded-2xl
}

.header-container {
    @apply basis-1/12 flex justify-around items-center
}

.select {
    @apply border-solid border-2 border-black p-4 rounded-full cursor-pointer
}


.button-container{
    @apply flex justify-center items-center p-4 gap-8
}

.date-button{

    @apply w-12 h-12 border-solid border-2 border-black rounded-full
}

.body-container {
    @apply basis-11/12 px-4 overflow-y-scroll border-solid border-t-2 border-black
}

.body-container::-webkit-scrollbar {
  display: none;
}

.body-container  {
  -ms-overflow-style: none;
  scrollbar-width: none;
}

</style>