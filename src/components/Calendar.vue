<template>
    <select v-model="selected">
        <option v-for="option in options" v-bind:value="option.value">
            {{ option.text }}
        </option>
    </select>
    <component v-bind:is="selected" />
  </template>
  
<script setup>

import { ref } from "vue";

import Day from './Day.vue';
import Week from './Week.vue';
import Month from './Month.vue';

const selected = ref(Day);

const options = [
        { text: 'Day', value: Day },
        { text: 'Week', value: Week },
        { text: 'Month', value: Month }
]

</script>

<style scoped>

</style>