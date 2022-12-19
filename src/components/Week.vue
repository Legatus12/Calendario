<template>
    <div class="week-container">
        <div class="day-container">
            <div class="day-header" v-for="day in week">
                {{ day.getDate() }}
            </div>
        </div>
        <div>
            <!--
            <div class="drop-zone" @drop="onDrop($event, 12)" @dragenter.prevent @dragover.prevent>
                <h1>12</h1>
                <div v-for="item in getList(12)" :key="item.id" class="drag-el" draggable="true"
                    @dragstart="startDrag($event, item)">
                    {{ item.title }}
                </div>
            </div>
            -->
        </div>
    </div>
</template>


<script setup>

import { ref } from 'vue';

const props = defineProps({
    loadedDay: Object
});


</script>


<style scoped>

.week-container{
    @apply w-full h-full flex flex-col
}

.day-container{
    @apply flex
}

.day-header{
    @apply w-full
}


</style>