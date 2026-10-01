<script setup>
defineProps(['conteudo'])
defineEmits(['clique'])
</script>
<template>
    <button @click="$emit('clique')">{{ conteudo }}</button>
</template>