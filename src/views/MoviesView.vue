<script setup>
import { ref, onMounted } from "vue";

let movies = ref([]);

function fetchMovies() {
    fetch("/api/v1/movies")

    .then(response => response.json())

    .then(data => {
        movies.value = data.movies;
    })

    .catch(error => {
        console.error("Error fetching movies:", error);
    });
}

onMounted(() => {
    fetchMovies();
});

</script>

<template>
    <div class="container mt-4">
        <h2>Movies</h2>
        <div class="row">
            <div v-for="movie in movies" :key="movie.id" class="col-md-4 mb-4">
                <div class="card">
                    <img :src="movie.poster" class="card-img-top" alt="Movie Poster" />
                    <div class="card-body">
                        <h5 class="card-title">{{ movie.title }}</h5>
                        <p class="card-text">{{ movie.description }}</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
