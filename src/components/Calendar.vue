<template>
    <div class="main-container">
        <div class="header-container">
            <div class="select-container">
                <select v-model="selectedComponent" class="select">
                    <option v-for="option in options" :value="option.value">
                        {{ option.text }}
                    </option>
                </select>
            </div>

            <div class="date-container">
                <button class="date-button" @click="getPrevious"><b>&lt;</b></button>

                <div>{{ date }}</div>
                <!--
                <div v-if="selectedComponent == Day">{{ date }}</div>

                <div v-if="selectedComponent == Week" class="">
                     {{ date }}
                </div>
                
                <div v-if="selectedComponent == Month">{{ date }}</div>
                -->
                <button class="date-button" @click="getNext"><b>&gt;</b></button>
            </div>

            <div class="option-container">
                <AddEvent />
            </div>
        </div>

        <div class="body-container">
            <component :is="selectedComponent" :range="null"/>
        </div>
    </div>
</template>
  
<script setup>

import { ref, shallowRef, watch } from "vue";

import Day from './Day.vue';
import Week from './Week.vue';
import Month from './Month.vue';

import AddEvent from "./AddEvent/AddEvent.vue";

import dayjs from 'dayjs';
import calendar from 'dayjs/plugin/calendar';

dayjs.extend(calendar);

//

const selectedComponent = shallowRef(Day);

const options = [
    { text: 'día', value: Day },
    { text: 'semana', value: Week },
    { text: 'mes', value: Month }
]

let date = ref(dayjs());

const getWeekRange = (date) => {
    let range = [];
    for (let i = 1; i <= 7; i++) {
        range.push(date.startOf('week').add(i, 'day'))
    }
    return range;
}

const getMonthRange = (date) => {
    let range = [];
    for (let i = 0; i < date.daysInMonth(); i++) {
        range.push(date.startOf('month').add(i, 'day'))
    }
    if(range[0].day() != 0){
        let i = 1;
        let aux = [];
        do {
            aux.push(range[0].startOf('week').add(i, 'day'));
            i++;
        } while (i != range[0].day());
        range = [...aux, ...range]
    }
    /*
    if(range[range.length - 1].day() != 6){
        console.log(range[range.length - 1])
        let i = 1;
        let aux = [];
        do {
            aux.push(range[0].add(i, 'day'));
            i++;
        } while (range[0].add(i, 'day').day() != 7);
        range = [...range, ...aux]
    }
    */
    return range;
}

console.log(getWeekRange(date.value))
console.log(getMonthRange(date.value))

const getPrevious = () => {
    switch (selectedComponent.value) {
        case Day:
            date.value = date.value.subtract(1, 'day');
            break;

        case Week:
            date.value = date.value.subtract(1, 'week');
            break;

        case Month:
            date.value = date.value.subtract(1, 'month');
            break;
    }
}

const getNext = () => {
    switch (selectedComponent.value) {
        case Day:
            date.value = date.value.add(1, 'day');
            break;

        case Week:
        date.value = date.value.add(1, 'week');
            break;

        case Month:
        date.value = date.value.add(1, 'month');
            break;
    }
}

watch(date,(nv) => {
    console.log(nv.$d);
})

</script>

<style scoped>
.main-container {
    @apply flex flex-col w-full h-full border-solid border-[1px] border-[#aeaeae] rounded-2xl bg-[#f6f6f6]
}

.header-container {
    @apply basis-1/12 flex justify-around items-center 
}

.select-container{
    @apply basis-1/3 p-4
}

.select {
    @apply px-6 py-4 rounded-full cursor-pointer bg-[#f6f6f6] hover:bg-[#d6d6d6]
}

option{
    background-color: #f6f6f6;
}

.date-container{
    @apply basis-1/3 flex justify-between items-center p-4 gap-8
}

.date-button{
    @apply px-6 py-4 rounded-full hover:bg-[#d6d6d6]
}

.option-container{
    @apply basis-1/3 flex justify-end p-4
}

.body-container {
    @apply basis-11/12 px-4 overflow-y-scroll border-solid border-t-[1px] border-[#aeaeae]
}

.body-container::-webkit-scrollbar {
  display: none;

}

.body-container  {
  -ms-overflow-style: none;
  scrollbar-width: none;
}

</style>