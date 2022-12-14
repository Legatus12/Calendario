<template>
    <div>
        <AddEvent @SendEvent="addEvent" />
    </div>
    <div class="flex flex-row">

        <div class="drop-zone" @drop="onDrop($event, 12)" @dragenter.prevent @dragover.prevent>
            <h1>12</h1>
            <div v-for="item in getList(12)" :key="item.id" class="drag-el" draggable="true"
                @dragstart="startDrag($event, item)">
                {{ item.title }}
            </div>
        </div>
        <div class="drop-zone" @drop="onDrop($event, 13)" @dragenter.prevent @dragover.prevent>
            <h1>13</h1>
            <div v-for="item in getList(13)" :key="item.id" class="drag-el" draggable="true"
                @dragstart="startDrag($event, item)">
                {{ item.title }}
            </div>
        </div>
        <div class="drop-zone" @drop="onDrop($event, 14)" @dragenter.prevent @dragover.prevent>
            <h1>14</h1>
            <div v-for="item in getList(14)" :key="item.id" class="drag-el" draggable="true"
                @dragstart="startDrag($event, item)">
                {{ item.title }}
            </div>
        </div>
        <div class="drop-zone" @drop="onDrop($event, 15)" @dragenter.prevent @dragover.prevent>
            <h1>15</h1>
            <div v-for="item in getList(15)" :key="item.id" class="drag-el" draggable="true"
                @dragstart="startDrag($event, item)">
                {{ item.title }}
            <button class="options">...</button>
            </div>
        </div>
    </div>
    <div class="flex justify-center  bg-blue-500 w-20 h-12 rounded-full" @drop="deleteEvent($event)" @dragenter.prevent
        @dragover.prevent>
        <h1 class="mt-3">P</h1>
    </div>

</template>

<script setup>

import AddEvent from './AddEvent.vue';
import axios from "axios";

import { onBeforeMount, ref } from "vue";


const items = ref([])

// método para cambiar el numero de la lista a la que pertenece según 
// donde se tira, se usa el dia del string 
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

    const itemID = event.dataTransfer.getData("itemID")
    const item = items.value.find((el) => el.id == itemID)

    try {
        //lo borramos en la base de datos y la lista que estaba
        await axios.delete(`http://localhost:3000/events/${item.id}`)
        item.list = null;
    } catch {
        alert("Problema al borrar evento");
    }
}

// cuando completamos el registro del formulario recibimos el evento en addEvent
const addEvent = (event) => {
    // se carga directamente en el array que corresponda y ya sale por pantalla 
    items.value.push({ id: event.id, title: event.name, list: changeList(event) });

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
.drop-zone {
    @apply w-32 h-36 m-auto bg-gray-500 p-3 min-h-min mb-10
}

.drag-el {
    @apply bg-blue-400 text-white p-1 mb-3 
}

.drag-el:nth-last-of-type(1) {
    @apply m-0
}
</style>

