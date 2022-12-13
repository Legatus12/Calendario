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
const db = ref([]);

const eventsInitials = () => {

    db.value.forEach(element => {

        items.value.push({ id: element.id, title: element.name, list: changeList(element) });
    });

}

const changeList = (element) => {
    return parseInt(element.start_date.substring(element.start_date.length - 2))
}

const getList = (list) => {
    return items.value.filter((el) => el.list == list)
}

const startDrag = (event, item) => {
    event.dataTransfer.dropEffect = "move"
    event.dataTransfer.effectAllowed = "move"
    event.dataTransfer.setData("itemID", item.id)
}

const onDrop = async (event, list) => {
    const itemID = event.dataTransfer.getData("itemID")
    const item = items.value.find((el) => el.id == itemID)
    item.list = list;

    try {
        await axios.patch(`http://localhost:3000/events/${item.id}`, {
            "start_date": db.value.reduce((acc, el) => {
                if (el.id == item.id)
                    acc += el.start_date.substring(0, el.start_date.length - 2) + item.list;

                return acc;
            }, "")
        });
    } catch (error) {
        alert("Problema al cambiar el evento")
    }
}

const loadEvent = async () => {

    try {
        const res = await axios.get(`http://localhost:3000/events/`);
        db.value = res.data;
        eventsInitials();
    } catch (error) {
        alert("Problema al traer eventos");
    }
}

const deleteEvent = async (event) => {
    const itemID = event.dataTransfer.getData("itemID")
    const item = items.value.find((el) => el.id == itemID)

    try {
        await axios.delete(`http://localhost:3000/events/${item.id}`)
        item.list = null;
    } catch {
        alert("Problema al borrar evento");
    }
}

const addEvent = (event) => {
    items.value.push({ id: event.id, title: event.name, list: changeList(event) });
    db.value.push(event);
}

onBeforeMount(() => {
    loadEvent();
})

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

