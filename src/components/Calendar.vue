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
                    {{ dayNames[date.subtract(1, 'day').day()] }}, {{ date.date() }} de {{ monthNames[date.month()] }}
                    de {{ date.year() }}
                </div>

                <div v-if="selectedComponent == Week">
                    {{ date.startOf('week').date() }}&nbsp;/&nbsp;{{ date.startOf('week').month() + 1 }}&nbsp;/&nbsp;{{
                        date.startOf('week').year()
                    }}
                    <br>
                    {{ date.endOf('week').date() }}&nbsp;/&nbsp;{{ date.endOf('week').month() + 1 }}&nbsp;/&nbsp;{{
                        date.endOf('week').year()
                    }}
                </div>

                <div v-if="selectedComponent == Month">
                    {{ monthNames[date.month()] }} de {{ date.year() }}
                </div>

                <button class="date-button" @click="getNext"><b>&gt;</b></button>
            </div>

            <div class="option-container">
                <AddEvent @SendEvent="addEvent" />
            </div>
        </div>

        <div class="component-container">
            <component :is="selectedComponent" :date="date" :week="week" :month="month" :dayNames="dayNames"
                :selectDay="checkSelectedComponent" :list="items" @SendEvent="modifyEvents" />
        </div>
    </div>
</template>
  
<script setup>

import { ref, shallowRef, watch, onMounted, onBeforeMount, computed, reactive } from "vue";

import Day from './Day.vue';
import Week from './Week.vue';
import Month from './Month.vue';

import AddEvent from "./EventManagement/AddEvent.vue";

import dayjs from 'dayjs';
import calendar from 'dayjs/plugin/calendar';
import updateLocale from 'dayjs/plugin/updateLocale';

import axios from "axios";

dayjs.extend(calendar);
dayjs.extend(updateLocale);

const localeObject = {
    name: 'es',
    weekStart: 1,
}

dayjs.locale('es-my-settings', localeObject);

const checkSelectedComponent = computed(() =>
    selectedComponent.value === Day
);


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

//variable de los contenedores
const items = reactive([])

onMounted(() => {
    getWeekRange(date.value);
    getMonthRange(date.value);
    selectedMonth.value = date.value.month();
})
// actualizar calendario 
onBeforeMount(() => {
    loadEvent();
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
    if (month.value[0].day() != 1) {
        let aux = [];
        for (let i = 0; i < month.value[0].subtract(1, 'day').day(); i++) {
            aux.push(month.value[0].startOf('week').add(i, 'day'));
        }
        month.value = [...aux, ...month.value];
    }
    //si el mes no acaba en domingo...
    if (month.value[month.value.length - 1].day() != 0) {
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

watch(date, (newDate) => {
    console.log(newDate);
    getWeekRange(newDate);
    getMonthRange(newDate);
    selectedMonth.value = newDate.month();
})

// metodos de gestion de eventos 

const loadEvent = async () => {

    try {

        //recojo la información de la base de datos
        const res = await axios.get(`http://localhost:3000/events/`);

        //recorro la información que hemos recibido y pusheo lo items en la listas según el dia
        res.data.forEach(element => {
            items.push({ id: element.id, title: element.name, reason: element.reason, start_time: element.start_time, list: changeList(element) });
        });

    } catch (error) {
        console.log(error)
        alert("Problema al traer eventos");
    }
}

const addEvent = (event) => {
    // se carga directamente en el array que corresponda y ya lo vemos por pantalla 
    items.push({ id: event.id, title: event.name, reason: event.reason, start_time: event.start_time, list: event.start_date });

}

const modifyEvents = (event) => {
    items.forEach((element, index) => {
        if (element.id == event.id) {
            items[index] = { id: event.id, title: event.name, reason: event.reason, start_time: event.start_time, list: event.start_date };
        }
    });
}

const changeList = (element) => {
    return element.start_date
}

</script>

<style scoped>
.main-container {
    @apply flex flex-col w-full h-full border-solid border-[1px] border-[#aeaeae] bg-[#f6f6f6] overflow-hidden
}

.header-container {
    @apply basis-1/12 flex justify-around items-center shrink
}

.select-container {
    @apply basis-1/4 p-4
}

.select {
    @apply px-6 py-4 rounded-full cursor-pointer bg-[#f6f6f6] hover:bg-[#d6d6d6]
}

option {
    background-color: #f6f6f6;
}

.date-container {
    @apply basis-1/2 flex justify-between items-center p-4 gap-8 text-center
}

.date-button {
    @apply px-6 py-4 rounded-full hover:bg-[#d6d6d6]
}

.option-container {
    @apply basis-1/4 flex justify-end p-4
}

.component-container {
    @apply overflow-y-scroll h-full border-solid border-t-[1px] border-[#aeaeae]
}

.component-container::-webkit-scrollbar {
    display: none;
}

.component-container {
    -ms-overflow-style: none;
    scrollbar-width: none;
}
</style>