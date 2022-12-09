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
                <div>{{ selectedDate }}</div>
                <button class="date-button" @click="getNextDay">&gt;</button>
            </div>
        </div>

        <div class="body-container">
            <component v-bind:is="selectedComponent" class="component"/>
        </div>
    </div>
</template>
  
<script setup>

import { ref } from "vue";

import Day from './Day.vue';
import Week from './Week.vue';
import Month from './Month.vue';

const selectedComponent = ref(Day);
const options = [
        { text: 'Day', value: Day },
        { text: 'Week', value: Week },
        { text: 'Month', value: Month }
]

const currentDate = (new Date());
const selectedDate = ref(new Date());

const getPreviousDay = () => {
    selectedDate.value.setDate(selectedDate.value.getDate() - 1);
    console.log(selectedDate.value)
}

const getNextDay = () => {
    selectedDate.value.setDate(selectedDate.value.getDate() + 1);
    console.log(selectedDate.value)
}

</script>

<style scoped>

.main-container{
    @apply flex flex-col w-full h-full border-solid border-2 border-black rounded-2xl
}

.header-container{
    @apply flex justify-around items-center
}

.select{
    @apply border-solid border-2 border-black p-4 rounded-full cursor-pointer
}

.button-container{
    @apply flex justify-center items-center p-4 gap-8
}

.date-button{
    @apply w-12 h-12 border-solid border-2 border-black rounded-full
}

.body-container{
    @apply h-full w-full p-4
}

.component{
    @aplly border-solid border-2 border-black rounded-full
}

</style>