<script setup>
defineProps(['title', 'poster_path'])
</script>
<template>
    <div class="flex flex-col h-80 max-w-40 p-4 rounded-2xl bg-gray-950">
        <img :src="`https://image.tmdb.org/t/p/w500/${poster_path}`" :alt="title">
        <div class="flex items-end h-full justify-center text-center">
            <p class="text-wrap max-w-40 text-red-600">{{ title }}</p>
        </div>
    </div>
</template>