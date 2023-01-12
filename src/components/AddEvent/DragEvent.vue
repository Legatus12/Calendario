<template>
    <div class="drop-zone" @drop="onDrop($event, day)" @dragenter.prevent @dragover.prevent>
        <div v-for="item in getList(day)" :key="item.id" class="drag-el" draggable="true"
            @dragstart="startDrag($event, item)">
            <p class="w-full">{{ item.title }}</p>
            <OptionEvent @SendDelete="deleteEvent(item)" @SendEdit="editEvent" />
            <FormEvent :dialog="dialog" @CloseModal="closeModal" @SendEvent="" />
        </div>
    </div>
    <!-- Papelera donde lanza objeto a borrar <div class="flex justify-center  bg-blue-500 w-20 h-12 rounded-full"
        @drop="deleteEvent($event)" @dragenter.prevent @dragover.prevent>
        <h1 class="mt-3">P</h1>
        </div>
        -->
</template>

<script setup>

import OptionEvent from './OptionEvent.vue';
import FormEvent from './FormEvent.vue';

import axios from "axios";
import { ref } from "vue";

const dialog = ref();

const props = defineProps({ day: Number, list: Array });


const closeModal = (boolean) => {
    dialog.value = boolean;
}

// método para cambiar el numero de la lista a la que pertenece según 
// donde se tira, se usa el dia del string 

//metodos de uso propios de draggable
const getList = (list) => {
    //cambio al lista que recibo
    return props.list.filter((el) => el.list == list)
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
    const item = props.list.find((el) => el.id == itemID)
    item.list = list;

    try {
        //buscamos el item en la base de datos y lo actualizamos con la fecha segundo donde se ha tirado
        const res = await axios.get(`http://localhost:3000/events/${item.id}`);

        await axios.patch(`http://localhost:3000/events/${item.id}`, {
            "start_date": list
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

const editEvent = () => {
    dialog.value = true;
}

</script>

<style scoped>

.drop-zone {
    @apply w-full h-full bg-[#f6f6f6] pt-2 px-2 flex flex-col gap-2 overflow-y-scroll
}

.drag-el {
    @apply flex justify-between items-center border-solid border-[1px] border-[#888888] text-[#232323] text-xl font-bold p-2 rounded-xl
}

.drag-el:nth-last-of-type(1) {
    @apply m-0
}

.drop-zone::-webkit-scrollbar {
    display: none;
    overflow-y: scroll;
}

.drop-zone {
    -ms-overflow-style: none;
    scrollbar-width: none;
}

</style>

