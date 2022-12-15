<template>
    <div>
        <button type="button" @click="sendDelete"
            class="w-3 rounded-full border border-transparent  bg-red-600 px-4 py-2 text-base font-medium text-white shadow-sm hover:bg-red-300 focus:outline-none"></button>
        <button type="button" @click="sendEdit"
            class="w-3 rounded-full border border-transparent bg-green-600 px-4 py-2 text-base font-medium text-white shadow-sm hover:bg-green-300 focus:outline-none"></button>
    </div>
</template>

<script setup>

const emits = defineEmits(["SendDelete", "SendEdit"])

const sendDelete = () => {
    emits("SendDelete");
}

const sendEdit = () => {
    emits("SendEdit");
}

</script>

<style scoped>

</style>