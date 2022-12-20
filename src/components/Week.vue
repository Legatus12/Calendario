<template>
    <div class="week-container">
        <div class="day-container">
            <div class="day-column" v-for="day in week">
                <div class="day-header">
                    <div class="current"
                    v-if="day.date() == selectedDay && day.month() == currentMonth" >
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
                <div class="day-body">

                </div>
            </div>
        </div>
    </div>
</template>


<script setup>

import { ref } from 'vue';

const props = defineProps({
    week: Array,
    dayNames: Array,
    currentMonth: Number,
    selectedDay: Number
});


</script>


<style scoped>

.week-container{
    @apply w-full h-full flex flex-col
}

.day-container{
    @apply w-full h-full flex gap-[1px] bg-[#aeaeae] border-solid border-b-[1px] border-[#aeaeae]
}

.day-column{
    @apply w-full h-full bg-[#aeaeae] flex flex-col gap-[1px]
}

.day-header{
    @apply flex flex-col h-24 items-center justify-center p-2 text-center bg-[#f6f6f6]
}

.day-body{
    @apply w-full h-full bg-[#f6f6f6]
}

.current{
    @apply w-fit bg-[#f54f59] text-[#f6f6f6] font-black p-2 rounded-2xl
}

</style>