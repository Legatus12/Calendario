<template>
    <div class="flex justify-center">
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
                            class="relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-lg ">
                            <div class=" px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
                                <div class="sm:flex sm:items-start">
                                    <div
                                        class="mx-auto flex h-12 w-12 flex-shrink-0 items-center justify-center rounded-full  sm:mx-0 sm:h-10 sm:w-10">


                                    </div>
                                    <div class="w-full text-center">
                                        <div>
                                            <form class="flex flex-col" @submit.prevent="addEvent">
                                                <label for="name">Nombre evento</label><br>
                                                <input v-model="event.name" class="border border-blue-600 mx-10 rounded"
                                                    type="text" id="nombre" required><br>
                                                <label for="description">Descripcion</label><br>
                                                <input v-model="event.description"
                                                    class="border border-blue-600 mx-10 rounded" type="text"
                                                    id="description" required><br>
                                                <div class="flex flex-row ">
                                                    <button type="button" @click="dialog = false"
                                                        class="mt-10 w-full justify-center rounded-md border border-transparent  bg-red-600 px-4 py-2 text-base font-medium text-white shadow-sm hover:bg-red-300 focus:outline-none">Cancel</button>
                                                    <button type="submit"
                                                        class="mt-10 w-full justify-center rounded-md border border-transparent bg-blue-600 px-4 py-2 text-base font-medium text-white shadow-sm hover:bg-blue-300 focus:outline-none">Aceptar</button>
                                                </div>
                                            </form>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <!--
                            <div class=" px-4 py-3 sm:flex sm:flex-row-reverse sm:px-6">
                                <button type="button" @click="dialog = false"
                                    class="inline-flex w-full justify-center rounded-md border border-transparent bg-blue-600 px-4 py-2 text-base font-medium text-white shadow-sm hover:bg-blue-300 focus:outline-none sm:ml-3 sm:w-auto sm:text-sm">aceptar</button>
                                <button type="button" @click="dialog = false"
                                    class="mt-3 inline-flex w-full justify-center rounded-md border border-transparent  bg-red-600 px-4 py-2 text-base font-medium text-white shadow-sm hover:bg-red-300 focus:outline-none sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm">Cancel</button>
                            </div>
                            -->
                        </div>
                    </transition>
                </div>
            </div>
        </div>
    </transition>
</template>

<script setup>

import { ref, reactive } from 'vue';

const dialog = ref();
const event = reactive({ name: "", description: "" });
const emits = defineEmits(["SendEvent"])



const addEvent = () => {

    if (event.name && event.description) {
        emits("SendEvent", event);
        console.log(event)
        event.name = "";
        event.description = "";
        console.log(event)
        dialog.value = false;
    } else {
        alert('Campos obligatorios')

    }
}

</script>

<style lang="scss" scoped>

</style> 