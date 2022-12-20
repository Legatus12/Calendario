<template>
    <div class="week-container">
        <div class="day-container">
            <div class="day-column" v-for="day in week">
                <div class="day-header">
                    <div class="current"
                    v-if="day.date() == selectedDay && day.month() == currentMonth" >
                        <div>
                            {{ dayNames[day.subtract(1, 'day').day()] }}
                        </div>
                        <div>
                            {{ day.date() }}
                        </div>
                    </div>
                    <div v-else>
                        <div>
                            {{ dayNames[day.subtract(1, 'day').day()] }}
                        </div>
                        <div>
                            {{ day.date() }}
                        </div>
                    </div>
                </div>
                <!--drop zone-->
                <div class="day-body" @drop="onDrop($event, day.date())" @dragenter.prevent @dragover.prevent>
                    <div v-for="item in getList(day.date())" :key="item.id" class="item" draggable="true"
                        @dragstart="startDrag($event, item)">
                        {{ item.title }}
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>


<script setup>

import { ref, onBeforeMount } from 'vue';

import AddEvent from './AddEvent/AddEvent.vue';
import OptionEvent from './AddEvent/OptionEvent.vue';
import FormEvent from './AddEvent/FormEvent.vue';

import axios from "axios";

const props = defineProps({
    week: Array,
    dayNames: Array,
    currentMonth: Number,
    selectedDay: Number
});

//moviendo

const items = ref([])
const dialog = ref();

const closeModal = (boolean) => {
    dialog.value = boolean;
}

// método para cambiar el numero de la lista a la que pertenece según donde se tira, se usa el dia del string 
const changeList = (element) => {
    return parseInt(element.start_date.substring(element.start_date.length - 2))
}
//metodos de uso propios de draggable
const getList = (list) => {
    return items.value.filter((el) => el.list == list)
}

//inicio del drag
const startDrag = (event, item) => {
    event.dataTransfer.dropEffect = "move"
    event.dataTransfer.effectAllowed = "move"
    event.dataTransfer.setData("itemID", item.id)
}

// cuando se suelta el drag
const onDrop = async (event, list) => {
    //getData método propio del dragable para obtener la id del objeto 
    const itemID = event.dataTransfer.getData("itemID")
    // buscamos y encontramos el item que corresponde a la id
    const item = items.value.find((el) => el.id == itemID)
    item.list = list;

    try {
        //buscamos el item en la base de datos y lo actualizamos con la fecha segundo donde se ha tirado
        const res = await axios.get(`http://localhost:3000/events/${item.id}`);

        await axios.patch(`http://localhost:3000/events/${item.id}`, {
            "start_date": res.data.start_date.substring(0, res.data.start_date.length - 2) + item.list
        });
    } catch (error) {
        console.log(error)
        alert("Problema al cambiar el evento")
    }
}

// Para manejar lo eventos añadir, borrar  y modificar

// Cuando cae un evento en la papelera
const deleteEvent = async (event) => {


    try {
        //lo borramos en la base de datos y la lista que estaba
        await axios.delete(`http://localhost:3000/events/${event.id}`)
        //null en list elimina el elemento del contenedor e id a null para evitar repetir el id
        event.list = null;
        event.id = null;
        alert("Evento borrado con exito")
    } catch {
        alert("Problema al borrar evento");
    }
}

// cuando completamos el registro del formulario recibimos el evento en addEvent
const addEvent = (event) => {
    // se carga directamente en el array que corresponda y ya lo vemos por pantalla 
    items.value.push({ id: event.id, title: event.name, list: changeList(event) });

}

const editEvent = () => {
    dialog.value=true;
}

//  Para mantener la pagina actualiza (al arrancar) con los eventos de la base de datos 

onBeforeMount(() => {
    loadEvent();
})

const loadEvent = async () => {

    try {

        //recojo la información de la base de datos
        const res = await axios.get(`http://localhost:3000/events/`);

        //recorro la información que hemos recibido y pusheo lo items en la listas según el dia
        res.data.forEach(element => {
            items.value.push({ id: element.id, title: element.name, list: changeList(element) });
        });

    } catch (error) {
        console.log(error)
        alert("Problema al traer eventos");
    }
}

</script>


<style scoped>

.week-container{
    @apply w-full h-full flex flex-col
}

.day-container{
    @apply w-full h-full flex gap-[1px] bg-[#aeaeae] border-solid border-b-[1px] border-[#aeaeae]
}

.day-column{
    @apply w-full h-full bg-[#aeaeae] flex flex-col gap-[1px]
}

.day-header{
    @apply flex flex-col h-24 items-center justify-center p-2 text-center bg-[#f6f6f6]
}

.day-body{
    @apply w-full h-full bg-[#f6f6f6] p-2
}

.current{
    @apply w-fit bg-[#f54f59] text-[#f6f6f6] font-black p-2 rounded-2xl
}

</style>