<template>
    <div class="day-container"> 
        <div class="period-container">
            <div class="period" v-for="i in 24">
                {{ (i - 1).toString().padStart(2, '0') }}
            </div>
        </div>
        <div class="period-container">
            <div class="period" v-for="i in 24"></div>
        </div>
    </div>
    <br>
</template>

<script setup>



</script>

<style scoped>

.day-container{
    @apply flex
}

.period-container{
    @apply basis-11/12 flex flex-col shrink-0
}

.period{
    @apply h-16 border-solid border-b-[1px] border-[#aeaeae]
}

</style>