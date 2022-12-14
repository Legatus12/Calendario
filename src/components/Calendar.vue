<template>
    <div class="main-container">
        <div class="header-container">
            <select v-model="selectedComponent" class="select">
                <option v-for="option in options" :value="option.value">
                    {{ option.text }}
                </option>
            </select>

            <div class="button-container">
                <button class="date-button" @click="getPrevious">&lt;</button>

                <div v-if="selectedComponent == Day">{{ selectedDate.day }} / {{ selectedDate.month + 1 }} / {{ selectedDate.year }}</div>

                <div v-if="selectedComponent == Week">
                    {{ selectedDate.weekStart }} <br> {{ selectedDate.weekEnd }}
                </div>
                
                <div v-if="selectedComponent == Month">{{ selectedDate.month + 1 }} / {{ selectedDate.year }}</div>

                <button class="date-button" @click="getNext">&gt;</button>
            </div>
        </div>
        
        <div class="body-container">
            <component :is="selectedComponent" />
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

let startAux = new Date();
startAux.setDate(startAux.getDate()-startAux.getDay()+1)
let endAux = new Date();
endAux.setDate(endAux.getDate()-endAux.getDay()+7)

const selectedDate = reactive({ 
    Date: currentDate, 
    day: currentDate.getDate(), 
    month: currentDate.getMonth(), 
    year: currentDate.getFullYear(), 
    weekStart: startAux,
    weekEnd: endAux
});

const getPrevious = () => {

    switch (selectedComponent.value) {
        case Day:
            selectedDate.Date.setDate(selectedDate.Date.getDate() - 1);
            break;

        case Week:
            selectedDate.Date.setDate(selectedDate.Date.getDate() - 7, selectedDate.Date.getMonth(), selectedDate.Date.getFullYear());
            break;

        case Month:
            selectedDate.Date.setDate(1);
            selectedDate.Date.setMonth(selectedDate.Date.getMonth()-1);
            break;
    
        default:
            break;
    }

    selectedDate.day = selectedDate.Date.getDate();
    selectedDate.month = selectedDate.Date.getMonth();
    selectedDate.year = selectedDate.Date.getFullYear();
    startAux = new Date(selectedDate.Date)
    startAux.setDate(startAux.getDate()-startAux.getDay()+1)
    selectedDate.weekStart = new Date(startAux);
    endAux = new Date(selectedDate.Date)
    endAux.setDate(endAux.getDate()-endAux.getDay()+7)
    selectedDate.weekEnd = new Date(endAux);
    console.log(selectedDate.Date)
}

const getNext = () => {

    switch (selectedComponent.value) {
        case Day:
            selectedDate.Date.setDate(selectedDate.Date.getDate() + 1);
            break;

        case Week:
            selectedDate.Date.setDate(selectedDate.Date.getDate() + 7, selectedDate.Date.getMonth(), selectedDate.Date.getFullYear());
            break;

        case Month:
            selectedDate.Date.setDate(1);
            selectedDate.Date.setMonth(selectedDate.Date.getMonth()+1);
            break;
    
        default:
            break;
    }

    selectedDate.day = selectedDate.Date.getDate();
    selectedDate.month = selectedDate.Date.getMonth();
    selectedDate.year = selectedDate.Date.getFullYear();
    startAux = new Date(selectedDate.Date)
    startAux.setDate(startAux.getDate()-startAux.getDay()+1)
    selectedDate.weekStart = new Date(startAux);
    endAux = new Date(selectedDate.Date)
    endAux.setDate(endAux.getDate()-endAux.getDay()+7)
    selectedDate.weekEnd = new Date(endAux);
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