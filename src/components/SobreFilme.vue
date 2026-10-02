<script setup>
import { ref, onMounted } from 'vue';
import Botao from './Botao.vue';
import CardMovie from './CardMovie.vue';

const movies = ref([])
const maxPages = ref(500)
const page = ref(1)
const maxVisibleButtons = 5
let pagination = []

const options = {
    method: "GET",
    headers: { accept: "application/json", Authorization: "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJjMGQ4NWU1OWE1YTQwZDU1ZGJjY2JmMDkxZGE3NDJkOCIsIm5iZiI6MTc5MDgyNjUyMC41MzEwMDAxLCJzdWIiOiI2YWJkZDgxODdhY2IyNGEwYWI4Zjk3MTkiLCJzY29wZXMiOlsiYXBpX3JlYWQiXSwidmVyc2lvbiI6MX0.JA5U3wEerkXBSlFDnX9tzGmcOlgHjhBpb_iakqLQ7ew" }
};
const listaFilmes = async (reqPage = page.value) => {
    const res = await fetch(`https://api.themoviedb.org/3/discover/movie?include_adult=false&include_video=false&language=pt-BR&page=${reqPage}&sort_by=popularity.desc`, options);
    const dados = await res.json();
    movies.value = dados.results;
    console.log(dados)
}

onMounted(() => {

    listaFilmes();
    genPagination();

})
const nextPage = () => {
    if (page.value < maxPages.value) {
        page.value++
        listaFilmes(page.value)
    }

    genPagination()
}

const prevPage = () => {
    if (page.value > 1) {
        page.value--
        listaFilmes(page.value)
    }
    genPagination()
}
const firstPage = () => {
    page.value = 1
    listaFilmes(page.value)
    genPagination()
}
const lastPage = () => {
    page.value = maxPages.value
    listaFilmes(page.value)
    genPagination()
}


const genPagination = () => {
    pagination = []
    let maxLeft = page.value - Math.floor(maxVisibleButtons - 3);
    let maxRight = page.value + Math.floor(maxVisibleButtons - 3);

    if (maxLeft < 1) {
        maxLeft = 1;
        maxRight = maxVisibleButtons;
    }
    if (maxRight > maxPages) {
        maxLeft = maxPages - (maxVisibleButtons - 1)
        maxRight = maxPages
        if (maxLeft < 1) {
            maxLeft = 1;
        }
    }
    for (let i = maxLeft; i <= maxRight; i++) {
        pagination.push(i);
    }
}
</script>
<template>
    <div class="flex flex-wrap gap-3 p-2 justify-center">
        <div v-for="movie in movies" :key="movie.id" class="flex flex-col">
            <CardMovie :title="`${movie.title}`" :poster_path="`${movie.poster_path}`"></CardMovie>
        </div>
    </div>
    <div class="flex gap-4 justify-center">
        <Botao @clique="firstPage" conteudo="<<"></Botao>
        <Botao @clique="prevPage" conteudo="<"></Botao>
        <span v-for="prevPagination in pagination">
            <span v-if="prevPagination != page" class="text-red-400">{{ prevPagination }}</span>
            <span class="text-green-500" v-else>{{ page }}</span>
        </span>
        <Botao @clique="nextPage" conteudo=">"></Botao>
        <Botao @clique="lastPage" conteudo=">>"></Botao>
    </div>
</template>