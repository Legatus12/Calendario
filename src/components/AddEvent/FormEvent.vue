<template>
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
                        <div class="relative transform overflow-hidden border-solid border-2 border-[#d6d6d6] bg-[#f6f6f6] text-[#232323] font-normal text-left shadow-xl transition-all  sm:my-8 sm:w-full sm:max-w-lg ">
                            <div class=" mx-10 px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
                                <div class="sm:flex sm:items-start">
                                    <div class="w-full text-center">
                                        <div>
                                            <form class="flex flex-col" @submit.prevent="sendEvent">
                                                <label for="name" class="text-left">Título</label><br>
                                                <input v-model="event.name" class="border border-[#d6d6d6] p-2  rounded"
                                                    type="text" id="name" required><br>
                                                <label for="reason" class="text-left">Motivo</label><br>
                                                <input v-model="event.reason" class="border border-[#d6d6d6] p-2 rounded"
                                                    type="text" id="reason" required><br>
                                                <label for="description" class="text-left">Descripción</label><br>
                                                <input v-model="event.description" class="border border-[#d6d6d6] p-2 rounded" type="text" id="description"
                                                    required><br>

                                                <div class="mt-5">

                                                    <input v-model="event.start_date"
                                                        class="p-2 border border-[#d6d6d6] rounded" type="date"
                                                        id="start" required>
                                                    <input v-model="event.start_time"
                                                        class="p-2 border border-[#d6d6d6] ml-5 rounded" type="time"
                                                        id="start" required>
                                                </div>

                                                <!-- Fecha fin para una futura funcionalidad 
                                                <div class="mt-5">
                                                    <label class="mr-9" for="end">Fin:</label>


                                                    <input v-model="event.end_date"
                                                        class="w-32 h-7 border border-[#d6d6d6]  rounded" type="date"
                                                        id="end" required>
                                                    <input v-model="event.end_time"
                                                        class="w-20 h-7 border border-[#d6d6d6] ml-5 rounded" type="time"
                                                        id="end" required><br>
                                                </div>
                                                -->

                                                <div class="flex gap-4 flex-row ">
                                                    <button type="button" @click="closeModal"
                                                        class="mt-10 w-full justify-center rounded-md border border-transparent bg-[#aeaeae] px-4 py-2 text-base font-medium text-[#232323] shadow-sm hover:bg-[#d6d6d6] focus:outline-none">Cancelar</button>
                                                    <button type="submit"
                                                        class="mt-10 w-full justify-center rounded-md border border-transparent bg-[#646464] px-4 py-2 text-base font-medium text-white shadow-sm hover:bg-[#424242] focus:outline-none">Aceptar</button>
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

import { reactive } from 'vue';

const event = reactive({ name: "", reason: "", description: "", start_date: "", start_time: "" });

const props = defineProps({
    dialog: {
        type: Boolean,
    },
})

const emits = defineEmits(["CloseModal","SendEvent"])


const closeModal = () =>{
    emits("CloseModal",false)
}

const sendEvent = () =>{
    emits("SendEvent",event)

}

</script>

<style  scoped>

</style>