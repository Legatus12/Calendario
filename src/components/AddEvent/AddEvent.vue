<template>
    <div class="flex justify-end mr-10 mb-10">
        <button @click="dialog = true" class="rounded-full bg-blue-600 text-white text-5xl w-12 h-12 hover:bg-blue-300"
            type="button">+</button>
    </div>

    <transition enter-active-class="ease-out duration-300" enter-class="opacity-0" enter-to-class="opacity-100"
        leave-active-class="ease-in duration-200" leave-class="opacity-100" leave-to-class="opacity-0">
        <div class="relative z-10" aria-labelledby="modal-title" role="dialog" aria-modal="true" v-show="dialog">
            <!--
    Background backdrop, show/hide based on modal state.

    Entering: "ease-out duration-300"
      From: "opacity-0"
      To: "opacity-100"
    Leaving: "ease-in duration-200"
      From: "opacity-100"
      To: "opacity-0"
  -->
            <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"></div>

            <div class="fixed inset-0 z-10 overflow-y-auto">
                <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
                    <!--
        Modal panel, show/hide based on modal state.

        Entering: "ease-out duration-300"
          From: "opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
          To: "opacity-100 translate-y-0 sm:scale-100"
        Leaving: "ease-in duration-200"
          From: "opacity-100 translate-y-0 sm:scale-100"
          To: "opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
      -->
                    <transition enter-active-class="ease-out duration-300"
                        enter-class="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
                        enter-to-class="opacity-100 translate-y-0 sm:scale-100"
                        leave-active-class="ease-in duration-200" leave-class="opacity-100 translate-y-0 sm:scale-100"
                        leave-to-class="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95">
                        <div
                            class="relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl transition-all  sm:my-8 sm:w-full sm:max-w-lg ">
                            <div class=" mx-10 px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
                                <div class="sm:flex sm:items-start">
                                    <div class="w-full text-center">
                                        <div>
                                            <form class="flex flex-col" @submit.prevent="addEvent">
                                                <label for="name">Nombre:</label><br>
                                                <input v-model="event.name" class="border border-blue-600  rounded"
                                                    type="text" id="name" required><br>
                                                <label for="reason">Motivo:</label><br>
                                                <input v-model="event.reason" class="border border-blue-600 rounded"
                                                    type="text" id="reason" required><br>
                                                <label for="description">Descripción:</label><br>
                                                <input v-model="event.description"
                                                    class="border border-blue-600 rounded" type="text" id="description"
                                                    required><br>

                                                <div class="mt-5">
                                                    <label class="mr-5" for="start">Inicio:</label>

                                                    <input v-model="event.start_date"
                                                        class="w-32 h-7 border border-blue-600 rounded" type="date"
                                                        id="start" required>
                                                    <input v-model="event.start_time"
                                                        class="w-20 h-7 border border-blue-600 ml-5 rounded" type="time"
                                                        id="start" required>
                                                </div>
                                                <div class="mt-5">
                                                    <label class="mr-9" for="end">Fin:</label>


                                                    <input v-model="event.end_date"
                                                        class="w-32 h-7 border border-blue-600  rounded" type="date"
                                                        id="end" required>
                                                    <input v-model="event.end_time"
                                                        class="w-20 h-7 border border-blue-600 ml-5 rounded" type="time"
                                                        id="end" required><br>
                                                </div>

                                                <div class="flex flex-row ">
                                                    <button type="button" @click="dialog = false"
                                                        class="mt-10 w-full justify-center rounded-md border border-transparent  bg-red-600 px-4 py-2 text-base font-medium text-white shadow-sm hover:bg-red-300 focus:outline-none">Cancelar</button>
                                                    <button type="submit"
                                                        class="mt-10 w-full justify-center rounded-md border border-transparent bg-blue-600 px-4 py-2 text-base font-medium text-white shadow-sm hover:bg-blue-300 focus:outline-none">Aceptar</button>
                                                </div>
                                            </form>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </transition>
                </div>
            </div>
        </div>
    </transition>
</template>

<script setup>

import { ref, reactive } from 'vue';
import axios from "axios";

const dialog = ref();
const event = reactive({ name: "", reason: "", description: "", start_date: "", start_time: "", end_date: "", end_time: "" });

const emits = defineEmits(["SendEvent"])



const addEvent = async () => {
    /*    se puede pasar de string a formato fecha
    const prueba = new Date("2022-12-12");
            console.log(prueba);*/
    try {
        if (event.name && event.reason && event.description && event.start_date && event.start_time && event.end_date && event.end_time) {
            const res = await axios.post(`http://localhost:3000/events/`, getEvent(event));
            // emits("SendEvent", event); futuro uso del programa

            alert("Evento registrado con éxito")

            event.name = "";
            event.reason = "";
            event.description = "";
            event.start_date = "";
            event.start_time = "";
            event.end_date = "";
            event.end_time = "";

            dialog.value = false;
        }
    } catch (error) {
        console.log(error);
        alert("Problema al registrar evento");
    }
}

const getEvent = (event) => {
    return {
        name: event.name, reason: event.reason, description: event.description, start_date:
            event.start_date, start_time: event.start_time, end_date: event.end_date, end_time: event.end_time
    }

}

</script>

<style scoped>

</style> 