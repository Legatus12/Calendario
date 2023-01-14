<template>
    <div>
        <div class="flex mt-5 mb-2">
            <div v-for="colorOption in colorOptions" :key="colorOption" :class="colorOption"
                class=" w-6 h-6 rounded-full mr-2 cursor-pointer border border-black" @click="changeColor(colorOption)">
            </div>
            <h1>{{currentColor}} seleccionado</h1>
        </div>
    </div>
</template>

<script setup>

import { ref } from "vue";

const props = defineProps({
    currentColor: {String, default:"Ningún color"}    
})

const emits = defineEmits(["SendColor"])

const color = ref("Ningún color");
const colorOptions = ref(['bg-red-500', 'bg-blue-500', 'bg-green-500', 'bg-yellow-500']);

function changeColor(newColor) {
    color.value = newColor.substring(newColor.indexOf("-")+1,newColor.lastIndexOf("-"));
    console.log(color.value);
    emits("SendColor", newColor);
}

</script>

<style scoped>

</style>