<template>
    <div class="month-container">
        <div class="header-container">
            <div class="day-header" v-for="index in 7">
                {{ dayNames[index-1] }}
            </div>
        </div>
        <div class="day-container">
            <div class="day" v-for="day in month">
                <div v-if="day.month() == selectedMonth" class="w-full h-full flex flex-col">
                    <div class="flex justify-between p-2">
                        <div v-if="day.date() == currentDay && day.month() == currentMonth" class="bg-[#f54f59] text-[#f6f6f6] font-black w-6 text-center rounded">
                            {{ day.date() }}
                        </div>
                        <div v-else class="">
                            {{ day.date() }}
                        </div>
                        <button class="hidden border-solid border-[1px] border-[#aeaeae] text-[#646464] px-2 rounded-full">+</button>
                    </div>
                    <!--drop zone-->
                    <DragEvent :list=list :day=day.date() class="drop-zone" /> <!-- pasar numero de day 20221220 y lista -->
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
    currentDay: Number,
    currentMonth: Number,
    list: Array,
});

</script>

<style scoped>

.month-container{
    @apply w-full h-full flex flex-col
}

.header-container{
    @apply flex gap-[1px] bg-[#aeaeae] border-solid border-b-[1px] border-[#aeaeae]
}

.day-header{
    @apply w-full bg-[#f6f6f6] text-center p-4
}

.day-container{
    @apply h-full grid grid-cols-7 grid-rows-6 gap-[1px] bg-[#aeaeae]
}

.day{
    @apply w-full bg-[#f6f6f6] 
}

.day:hover button{
    display: block;
}

</style>