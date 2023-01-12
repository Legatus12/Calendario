<template>
    <div>
        <button type="button" @click="sendEdit" class="w-8 h-8 rounded-full bg-[#424242] hover:bg-[#646464]"></button>
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