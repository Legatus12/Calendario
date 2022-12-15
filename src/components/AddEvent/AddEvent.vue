<template>
    <div class="flex justify-end mr-10 mb-10">
        <button @click="dialog = true" class="rounded-full bg-blue-600 text-white text-5xl w-12 h-12 hover:bg-blue-300"
            type="button">+</button>
    </div>

    <FormEvent :dialog="dialog" @CloseModal="closeModal" @SendEvent="receiveEvent" />

</template>

<script setup>

import FormEvent from './FormEvent.vue';

import { ref, reactive } from 'vue';
import axios from "axios";

const dialog = ref();
// no puedo poner const, no puedo igualar al evento con recibo con const
let event = reactive({ name: "", reason: "", description: "", start_date: "", start_time: "" });

const emits = defineEmits(["SendEvent"])


const closeModal = (boolean) => {
    dialog.value = boolean;
}

const receiveEvent = (events) => {
    console.log(events);
    event = events;
    addEvent();
}


//Tratamiento del evento 

const addEvent = async () => {
    /*    se puede pasar de string a formato fecha
    const prueba = new Date("2022-12-12");
            console.log(prueba);*/
    try {
        //si esta todo ok se sube a la BBDD
        if (event.name && event.reason && event.description && event.start_date && event.start_time) {
            const res = await axios.post(`http://localhost:3000/events/`, getEvent(event));
            //con todo se envia el evento con emit
            emits("SendEvent", res.data);

            alert("Evento registrado con éxito")

            event.name = "";
            event.reason = "";
            event.description = "";
            event.start_date = "";
            event.start_time = "";

            dialog.value = false;
        }
    } catch (error) {
        console.log(error);
        alert("Problema al registrar evento");
    }
}

//información que contiene el evento
const getEvent = (event) => {
    return {
        name: event.name, reason: event.reason, description: event.description, start_date:
            event.start_date, start_time: event.start_time
    }

}

</script>

<style scoped>

</style> 