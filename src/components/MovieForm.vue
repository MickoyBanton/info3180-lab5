<script setup>
import { ref, onMounted } from "vue";

const csrf_token = ref("");
const movieForm = ref(null);

onMounted(() => {
    getCsrfToken();
});

function getCsrfToken() {
    fetch('/api/v1/csrf-token')
    .then((response) => response.json())
    .then((data) => {
        csrf_token.value = data.csrf_token;
    })
    .catch((error) => console.error("Error fetching CSRF token", error));
}

function saveMovie() {
    if (!csrf_token.value) {
        console.error("CSRF token is not set.");
        return;
    }

    const form_data = new FormData(movieForm.value);

    fetch("/api/v1/movies", {
        method: 'POST',
        body: form_data,
        headers: {
            'X-CSRFToken': csrf_token.value
        }
    })
    .then(response => response.json())
    .then(data => {
        console.log(data);
    })
    .catch(error => {
        console.error(error);
    });
}
</script>

<template>
    <form ref="movieForm" @submit.prevent="saveMovie">
        <div class="form-group mb-3">
            <label for="title" class="form-label">Movie Title</label>
            <input type="text" name="title" class="form-control" />
        </div>

        <div class="form-group mb-3">
            <label for="description" class="form-label">Description</label>
            <input type="text" name="description" class="form-control" />
        </div>

        <div class="form-group mb-3">
            <label for="poster" class="form-label">Poster</label>
            <input type="file" name="poster" class="form-control" />
        </div>

        <button type="submit" class="btn btn-primary">Submit</button>
    </form>
</template>
