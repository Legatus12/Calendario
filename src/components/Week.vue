<template>
    <div class="week-container">
        <div class="day-container">
            <div class="day" v-for="day in week">
                <div class="header">
                    <div class="current" v-if="day.date() == currentDay && day.month() == currentMonth">
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
                <DragEvent :list=list :day=generateID(day) /> <!-- pasar numero de day 20221220 y lista -->
            </div>
        </div>
    </div>
</template>


<script setup>

import DragEvent from './AddEvent/DragEvent.vue';

const props = defineProps({
    week: Array,
    dayNames: Array,
    currentMonth: Number,
    currentDay: Number,
    list: Array,
});

const generateID = day => String(day.year()) + '-' + String(day.month() + 1).padStart(2, '0') + '-' + String(day.date()).padStart(2, '0');

</script>


<style scoped>
.week-container {
    @apply w-full h-full flex flex-col
}

.day-container {
    @apply w-full h-full flex gap-[1px] bg-[#aeaeae] border-solid border-b-[1px] border-[#aeaeae]
}

.day {
    @apply w-full h-full bg-[#aeaeae] flex flex-col gap-[1px] shrink
}

.header {
    @apply flex flex-col h-24 items-center justify-center p-2 text-center bg-[#f6f6f6] shrink
}

.current {
    @apply w-fit bg-[#f54f59] text-[#f6f6f6] font-black p-2 rounded-2xl
}

</style>