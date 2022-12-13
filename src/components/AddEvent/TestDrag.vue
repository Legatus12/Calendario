<template>
    <div>
        <AddEvent />
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
    <button type="button" @click="test"
        class="mt-10 w-full justify-center rounded-md border border-transparent  bg-blue-600 px-4 py-2 text-base font-medium text-white shadow-sm hover:bg-red-300 focus:outline-none">test</button>
    <button type="button"
        class="mt-10 w-full justify-center rounded-md border border-transparent  bg-red-600 px-4 py-2 text-base font-medium text-white shadow-sm hover:bg-red-300 focus:outline-none">Borrar</button>
</template>

<script setup>

import AddEvent from './AddEvent.vue';
import axios from "axios";

import { onBeforeMount, ref } from "vue";

const items = ref([])
const db = ref([]);

const eventsInitials = () => {

    db.value.forEach(element => {

        items.value.push({ id: element.id, title: element.name, list: parseInt(element.start_date.substring(element.start_date.length - 2)) });
    });

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
    console.log(item);

    await axios.patch(`http://localhost:3000/events/${item.id}`, {
        "start_date": db.value.reduce((acc, el) => {
            if (el.id == item.id)
                acc += el.start_date.substring(0, el.start_date.length - 2) + item.list;
            return acc;
        }, "")
    });
}

const loadEvent = async () => {

    try {
        const res = await axios.get(`http://localhost:3000/events/`);
        db.value = res.data;
        eventsInitials();
    } catch (error) {
        console.log(error);
        alert("Problema al traer registros");
    }
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

