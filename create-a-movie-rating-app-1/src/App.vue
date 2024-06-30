<script setup>
import { computed, ref } from "vue";
import { items } from "./movies.json";
import FormAddMovie from "./components/FormAddMovie.vue";
import MovieItem from "./components/MovieItem.vue";

const movies = ref(items);
const movieToEdit = ref(null);
const isAddMovieOpen = ref(false);

const handleClose = () => {
  isAddMovieOpen.value = false;
};

const handleEdit = (id) => {
  const index = movies.value.findIndex((movie) => movie.id === id);
  if (index !== -1) {
    const targetMovie = movies.value[index];
    // console.log(targetMovie);
    movieToEdit.value = targetMovie;
    isAddMovieOpen.value = true;
  } else {
    console.warn("Movie with ID", id, "not found"); // Handle no match scenario
  }
};

const handleDelete = (id) => {
  const filtered = movies.value.filter((movie) => movie.id != id);
  movies.value = filtered;
};

const averageRating = computed(() => {
  let averageRating = 0;
  for (let movie of movies.value) {
    averageRating += movie.rating;
  }
  return movies.value.length ? Math.round(averageRating / movies.value.length) : 0;
});
</script>

<template>
  <FormAddMovie v-if="isAddMovieOpen" :movies :handleClose="handleClose" :movie="movieToEdit" />
  <div class="flex flex-wrap items-center justify-between px-10">
    <div class="flex flex-col justify-start">
      <p class="w-auto text-white font-bold">Average Rating {{ averageRating }}</p>
      <p class="w-auto text-white font-bold">Number of Movies {{ movies.length }}</p>
    </div>
    <button class="px-3 py-1 bg-blue-500 text-white rounded" @click="isAddMovieOpen = !isAddMovieOpen">Add Movie</button>
  </div>
  <section class="flex flex-row flex-wrap gap-5 w-full pt-10 items-center justify-center pb-10">
    <MovieItem v-for="movie in movies" :key="movie.id" :movie :handleDelete :handleEdit />
  </section>
</template>
