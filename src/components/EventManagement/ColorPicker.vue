<template>
    <div>
        <div class="flex mt-5 mb-2">
            <div v-for="colorOption in colorOptions" :key="colorOption" :class="colorOption"
                class=" w-6 h-6 rounded-full mr-2 cursor-pointer border border-black" @click="changeColor(colorOption)">
            </div>
            <h1>{{colorSpanish[currentColor]}} seleccionado</h1>
        </div>
    </div>
</template>

<script setup>

import { ref, reactive } from "vue";

const props = defineProps({
    currentColor: {type:String, default:""}    
})

const emits = defineEmits(["SendColor"])

const colorSpanish = reactive({"":"Ningun Color","bg-red-500":"Rojo", 'bg-blue-500':"Azul", 'bg-green-500':"Verde",'bg-yellow-500':"Amarillo"});
const colorOptions = ref(['bg-red-500', 'bg-blue-500', 'bg-green-500', 'bg-yellow-500']);


function changeColor(newColor) {
        
    emits("SendColor", newColor);
}

</script>

<style scoped>

</style>