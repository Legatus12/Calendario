<template>
    <div class="month-container">
        <div class="day-container">
            <div class="day-header" v-for="index in 7">
                {{ dayNames[index-1] }}
            </div>
        </div>
        <div class="tile-container">
            <div class="tile" v-for="day in month">
                <div v-if="day.month() == selectedMonth" class="w-full h-full flex flex-col p-2">
                    <div v-if="day.date() == selectedDay && day.month() == currentMonth" class="bg-[#f54f59] text-[#f6f6f6] font-black w-6 text-center rounded">
                        {{ day.date() }}
                    </div>
                    <div v-else>
                        {{ day.date() }}
                    </div>
                    <!--drop zone-->
                    <DragEvent :list=list :day=day.date() /> <!-- pasar numero de day 20221220 y lista -->
                </div>
                <div v-else class="w-full h-full p-2 bg-[#d6d6d6]">
                    {{ day.date() }}
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>

import DragEvent from './AddEvent/DragEvent.vue';

const props = defineProps({
    month: Array,
    dayNames: Array,
    selectedMonth: Number,
    selectedDay: Number,
    currentMonth: Number,
    list: Array,
});

</script>

<style scoped>

.month-container{
    @apply w-full h-full flex flex-col
}

.day-container{
    @apply flex gap-[1px] bg-[#aeaeae] border-solid border-b-[1px] border-[#aeaeae]
}

.day-header{
    @apply w-full bg-[#f6f6f6] text-center p-4
}

.tile-container{
    @apply h-full grid grid-cols-7 grid-rows-6 gap-[1px] bg-[#aeaeae]
}

.tile{
    @apply w-full bg-[#f6f6f6]
}

</style>