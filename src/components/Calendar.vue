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

                <div v-if="selectedComponent == Day">
                    {{ dayNames[date.subtract(1, 'day').day()] }}, {{ date.date() }} de {{ monthNames[date.month()] }} de {{ date.year() }}
                </div>

                <div v-if="selectedComponent == Week" class="">
                    {{ date.startOf('week').date() }} de {{ monthNames[date.month()] }} de {{ date.year() }}
                </div>
                
                <div v-if="selectedComponent == Month">
                    {{ monthNames[date.month()] }} de {{ date.year() }}
                </div>
                
                <button class="date-button" @click="getNext"><b>&gt;</b></button>
            </div>

            <div class="option-container">
                <AddEvent />
            </div>
        </div>

        <div class="body-container">
            <component :is="selectedComponent" :dayNames="dayNames" :week="week" :month="month" :selectedMonth="selectedMonth" :selectedDay="selectedDay" :currentMonth="currentMonth" />
        </div>
    </div>
</template>
  
<script setup>

import { ref, shallowRef, watch, onMounted } from "vue";

import Day from './Day.vue';
import Week from './Week.vue';
import Month from './Month.vue';

import AddEvent from "./AddEvent/AddEvent.vue";

import dayjs from 'dayjs';
import calendar from 'dayjs/plugin/calendar';
import updateLocale from 'dayjs/plugin/updateLocale';

dayjs.extend(calendar);
dayjs.extend(updateLocale);

//

const localeObject = {
    name: 'es',
    weekStart: 1,
}

dayjs.locale('es-my-settings', localeObject);

console.log(dayjs().date())

//

const selectedComponent = shallowRef(Day);

const options = [
    { text: 'día', value: Day },
    { text: 'semana', value: Week },
    { text: 'mes', value: Month }
]

let date = ref(dayjs());

let week = ref([]);

let month = ref([]);

const selectedMonth = ref(0);

const selectedDay = ref(0);

const currentMonth = dayjs().month()

const dayNames = [
    "lunes",
    "martes",
    "miércoles",
    "jueves",
    "viernes",
    "sábado",
    "domingo"
]

const monthNames = [
    "enero",
    "febrero",
    "marzo",
    "abril",
    "mayo",
    "junio",
    "julio",
    "agosto",
    "septiembre",
    "octubre",
    "noviembre",
    "diciembre"
]

onMounted(() => {
    getWeekRange(date.value);
    getMonthRange(date.value);
    selectedMonth.value = (date.value.month());
    selectedDay.value = (date.value.date());
})

const getWeekRange = (date) => {
    week.value = [];
    for (let i = 0; i < 7; i++) {
        week.value.push(date.startOf('week').add(i, 'day'))
    }
}

const getMonthRange = (date) => {
    month.value = [];
    for (let i = 0; i < date.daysInMonth(); i++) {
        month.value.push(date.startOf('month').add(i, 'day'))
    }
    
    //si el mes no comienza en lunes...
    if(month.value[0].day() != 1){
        let aux = [];
        for (let i = 0; i < month.value[0].subtract(1, 'day').day(); i++) {
            aux.push(month.value[0].startOf('week').add(i, 'day'));
        }
        month.value = [...aux, ...month.value];
    }
    //si el mes no acaba en domingo...
    if(month.value[month.value.length - 1].day() != 0){
        let aux = [];
        for (let i = 0; i < 6 - month.value[month.value.length - 1].subtract(1, 'day').day(); i++) {
            aux.push(month.value[month.value.length - 1].add(i + 1, 'day'));
        }
        month.value = [...month.value, ...aux];
    }
}

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

watch(date,(newDate) => {
    console.log(newDate.$d);
    getWeekRange(newDate);
    getMonthRange(newDate);
    selectedMonth.value = newDate.month();
})

watch(selectedComponent,(nc) => {
    console.log(nc);
})

</script>

<style scoped>
.main-container {
    @apply flex flex-col w-full h-full border-solid border-[1px] border-[#aeaeae] bg-[#f6f6f6]
}

.header-container {
    @apply basis-1/12 flex justify-around items-center 
}

.select-container{
    @apply basis-1/4 p-4
}

.select {
    @apply px-6 py-4 rounded-full cursor-pointer bg-[#f6f6f6] hover:bg-[#d6d6d6]
}

option{
    background-color: #f6f6f6;
}

.date-container{
    @apply basis-1/2 flex justify-between items-center p-4 gap-8
}

.date-button{
    @apply px-6 py-4 rounded-full hover:bg-[#d6d6d6]
}

.option-container{
    @apply basis-1/4 flex justify-end p-4
}

.body-container {
    @apply overflow-y-scroll h-full border-solid border-t-[1px] border-[#aeaeae]
}

.body-container::-webkit-scrollbar {
  display: none;

}

.body-container  {
  -ms-overflow-style: none;
  scrollbar-width: none;
}

</style>