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
  const body = (document.querySelectorAll("body")[0].style.overflow = "auto");
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

const handleAddEdditMovie = () => {
  isAddMovieOpen.value = !isAddMovieOpen.value;
  const body = (document.querySelectorAll("body")[0].style.overflow = "hidden");
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
  <Transition name="bounce">
    <FormAddMovie class="z-50" v-if="isAddMovieOpen" :movies :handleClose="handleClose" :movie="movieToEdit" />
  </Transition>
  <div class="flex flex-wrap items-center justify-between px-10">
    <div class="flex flex-col justify-start">
      <p class="w-auto text-white font-bold">Average Rating {{ averageRating }}</p>
      <p class="w-auto text-white font-bold">Number of Movies {{ movies.length }}</p>
    </div>
    <button class="px-3 py-1 bg-blue-500 text-white rounded" @click="handleAddEdditMovie">Add Movie</button>
  </div>
  <main>
    <TransitionGroup class="flex flex-row flex-wrap gap-5 w-full pt-10 items-center justify-center pb-10" name="list" tag="ul">
      <MovieItem v-for="movie in movies" :key="movie.id" :movie :handleDelete :handleEdit />
    </TransitionGroup>
  </main>
</template>

<style scoped>
.bounce-enter-active {
  animation: bounce-in 0.5s;
}
.bounce-leave-active {
  animation: bounce-in 0.5s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.25);
  }
  100% {
    transform: scale(1);
  }
}

.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
</style>
