<template>
    <div class="week-container">
        <div class="day-container">
            <div class="day" v-for="day in week">
                <div class="header">
                    <div class="current" v-if="day.date() == dayjs().date() && day.month() == dayjs().month()">
                        <div>
                            {{ dayNames[day.subtract(1, 'day').day()] }}
                        </div>
                        <div>
                            {{ day.date() }}
                        </div>
                    </div>
                    <div v-else class="p-4">
                        <div>
                            {{ dayNames[day.subtract(1, 'day').day()] }}
                        </div>
                        <div>
                            {{ day.date() }}
                        </div>
                    </div>
                </div>
                <!--drop zone-->
                <DragEvent :list=list :day=generateID(day) />
            </div>
        </div>
    </div>
</template>


<script setup>

import DragEvent from './AddEvent/DragEvent.vue';

import dayjs from 'dayjs';

//

const props = defineProps({
    week: Array,
    dayNames: Array,
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
    @apply w-full h-full bg-[#aeaeae] flex flex-col gap-[1px]
}

.header {
    @apply w-full flex flex-col items-center justify-center text-center bg-[#f6f6f6]
}

.current {
    @apply w-full flex-col justify-center items-center bg-[#424242] text-[#f6f6f6] font-black p-4
}

</style>