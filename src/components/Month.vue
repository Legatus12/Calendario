<template>
    <div class="month-container">
        <div class="header-container">
            <div class="day-header" v-for="index in 7">
                {{ dayNames[index-1] }}
            </div>
        </div>
        <div class="day-container">
            <div class="day" v-for="day in month">
                <div v-if="day.month() == date.month()" class="current-month">
                    <div class="tile-header">
                        <div v-if="day.date() == dayjs().date() && day.month() == dayjs().month()" class="current-day">
                            {{ day.date() }}
                        </div>
                        <div v-else>
                            {{ day.date() }}
                        </div>
                        <!--
                        <button class="hidden border-solid border-[1px] border-[#aeaeae] text-[#646464] px-2 rounded-full">+</button>
                        -->
                    </div>
                    <!--drop zone-->
                    <DragEvent :list=list :day=generateID(day) @SendEvent="sendEvent" />
                </div>
                <div v-else class="not-current-month">
                    {{ day.date() }}
                </div>
            </div>
        </div>
    </div>
</template>


<script setup>

import DragEvent from './EventManagement/DragEvent.vue';

import dayjs from 'dayjs';

//

const emits = defineEmits(["SendEvent"])

const props = defineProps({
    date: Object,
    month: Array,
    dayNames: Array,
    list: Array,
});

const sendEvent = event => emits("SendEvent",event);

const generateID = day => String(day.year()) + '-' + String(day.month() + 1).padStart(2, '0') + '-' + String(day.date()).padStart(2, '0');

</script>


<style scoped>

.month-container{ @apply w-full h-full flex flex-col }

.current-month{ @apply w-full h-full flex flex-col }

.not-current-month{ @apply w-full h-full p-2 bg-[#d6d6d6] }

.header-container{ @apply flex gap-[1px] bg-[#aeaeae] border-solid border-b-[1px] border-[#aeaeae] }

.day-header{ @apply w-full bg-[#f6f6f6] text-center p-2 }

.day-container{ @apply w-full h-full grid grid-rows-6 grid-cols-7 gap-[1px] bg-[#aeaeae] shrink-0 }

.day{ @apply h-full shrink-0 bg-[#f6f6f6] overflow-y-hidden }
/*
.day:hover button{
    display: none;
}
*/
.current-day{ @apply font-black text-[#f6f6f6] bg-[#424242] px-2 rounded-lg }

.tile-header{ @apply flex justify-between p-2 }

</style>