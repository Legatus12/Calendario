<template>

    <button @click="dialog = true" class="option-button" type="button"> + añadir evento</button>
    <FormEvent :dialog="dialog" @CloseModal="closeModal" @SendEvent="receiveEvent" />

</template>

<script setup>

import FormEvent from './FormEvent.vue';
import { ref, reactive } from 'vue';
import axios from "axios";

const dialog = ref();
// no puedo poner const, no puedo igualar al evento con recibo con const
let event = reactive({ name: "", reason: "", start_date: "", start_time: "", color: "" });

const emits = defineEmits(["SendEvent"])


const closeModal = (boolean) => {
    dialog.value = boolean;

}

const receiveEvent = (events) => {
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
        if (event.name && event.reason && event.start_date && event.start_time) {
            const res = await axios.post(`http://localhost:3000/events/`, getEvent(event));
            //con todo se envia el evento con emit
            emits("SendEvent", res.data);

            alert("Evento registrado con éxito")

            event.name = "";
            event.reason = "";
            event.start_date = "";
            event.start_time = "";
            event.color = "";

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
        name: event.name, reason: event.reason, start_date:
            event.start_date, start_time: event.start_time, color: event.color
    }

}

</script>

<style scoped>
.option-button {
    @apply px-6 py-4 rounded-full hover:bg-[#d6d6d6]
}
</style> 