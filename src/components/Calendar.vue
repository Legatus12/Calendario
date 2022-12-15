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

                <div v-if="selectedComponent == Day">{{ selectedDate.day }} / {{ selectedDate.month + 1 }} / {{ selectedDate.year }}</div>

                <div v-if="selectedComponent == Week" class="">
                    {{ selectedDate.weekStart.getDate() }} / {{ selectedDate.weekStart.getMonth() + 1 }} / {{ selectedDate.weekStart.getFullYear() }} 
                    <br>
                     {{ selectedDate.weekEnd.getDate() }} / {{ selectedDate.weekEnd.getMonth() + 1 }} / {{ selectedDate.weekEnd.getFullYear() }} 
                </div>
                
                <div v-if="selectedComponent == Month">{{ selectedDate.monthName }} de {{ selectedDate.year }}</div>

                <button class="date-button" @click="getNext"><b>&gt;</b></button>
            </div>

            <div class="option-container">
                <button class="option-button"> + añadir evento</button>
            </div>
        </div>
        
        <div class="body-container">
            <component :is="selectedComponent" :loadedDays="selectedDate.Date "/>
        </div>
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
    { text: 'día', value: Day },
    { text: 'semana', value: Week },
    { text: 'mes', value: Month }
]

const currentDate = (new Date());

let startAux = new Date();
startAux.setDate(startAux.getDate()-startAux.getDay()+1);
let endAux = new Date();
endAux.setDate(endAux.getDate()-endAux.getDay()+7);

const selectedDate = reactive({ 
    Date: currentDate, 
    day: currentDate.getDate(), 
    month: currentDate.getMonth(),
    monthName: currentDate.toLocaleString('default', { month: 'long' }), 
    year: currentDate.getFullYear(), 
    weekStart: startAux,
    weekEnd: endAux
});

const getPrevious = () => {

    switch (selectedComponent.value) {
        case Day:
            selectedDate.Date.setDate(selectedDate.Date.getDate() - 1);
            //establece la fecha del día anterior
            break;

        case Week:
            selectedDate.Date.setDate(selectedDate.Date.getDate() - 7, selectedDate.Date.getMonth(), selectedDate.Date.getFullYear());
            //establece la fecha del día de la semana seleccionado actualmente de la semana anterior
            break;

        case Month:
            selectedDate.Date.setDate(1);
            selectedDate.Date.setMonth(selectedDate.Date.getMonth()-1);
            //establece la fecha del dia 1 del mes anterior
            // (dia 1 para evitar errores que se pueden producir navegando entre meses con distinto numero de días)
            break;
    
        default:
            break;
    }

    update();

    /**/console.log(selectedDate.Date);
}

const getNext = () => {

    switch (selectedComponent.value) {
        case Day:
            selectedDate.Date.setDate(selectedDate.Date.getDate() + 1);
            // establecemos la fecha del día siguiente
            break;

        case Week:
            selectedDate.Date.setDate(selectedDate.Date.getDate() + 7, selectedDate.Date.getMonth(), selectedDate.Date.getFullYear());
            // establecemos la fecha del día de la semana seleccionado actualmente de la semana siguiente
            break;

        case Month:
            selectedDate.Date.setDate(1);
            selectedDate.Date.setMonth(selectedDate.Date.getMonth()+1);
            // establecemos la fecha del dia 1 del mes siguiente
            // (dia 1 para evitar errores que se pueden producir navegando entre meses con distinto numero de días)
            break;
    
        default:
            break;
    }

    update();

    /**/console.log(selectedDate.Date);
}

const update = () => {
    selectedDate.day = selectedDate.Date.getDate();
    selectedDate.month = selectedDate.Date.getMonth();
    selectedDate.monthName = selectedDate.Date.toLocaleString('default', { month: 'long' });
    selectedDate.year = selectedDate.Date.getFullYear();
    startAux = new Date(selectedDate.Date);
    endAux = new Date(selectedDate.Date);
    startAux.setDate(startAux.getDate()-startAux.getDay()+1)
    selectedDate.weekStart = new Date(startAux);
    endAux.setDate(endAux.getDate()-endAux.getDay()+7)
    selectedDate.weekEnd = new Date(endAux);
}

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
    @apply p-4 rounded-full cursor-pointer bg-[#f6f6f6] hover:bg-[#d6d6d6]
}

option{
    background-color: #f6f6f6;
}

.date-container{
    @apply basis-1/3 flex justify-between items-center p-4 gap-8
}

.date-button{
    @apply w-12 h-12 rounded-full hover:bg-[#d6d6d6]
}

.option-container{
    @apply basis-1/3 flex justify-end p-4
}

.option-button{
    @apply px-4 h-12 rounded-full hover:bg-[#d6d6d6]
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