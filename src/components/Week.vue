<template>
    <div class="week-container">
        <div class="day-container">
            <div class="day-header"></div>
        </div>
        <div>
            {{ loadedDays }}
        </div>
    </div>
</template>


<script setup>

const props = defineProps({
    loadedDays: Date
});

</script>


<style scoped>

.week-container{
    @apply w-full h-full flex flex-col
}

</style>