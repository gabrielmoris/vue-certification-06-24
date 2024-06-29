<script setup>
import { ref } from "vue";
import { items } from "./movies.json";
import { StarIcon } from "@heroicons/vue/24/solid";
import FormAddMovie from "./components/FormAddMovie.vue";

const movies = ref(items);
const isAddMovieOpen = ref(false);

const handleStar = (movieID, n) => {
  movies.value[movieID - 1].rating = n;
};

const handleClose = () => {
  isAddMovieOpen.value = false;
};
</script>

<template>
  <button class="px-3 absolute top-5 right-5 py-1 bg-blue-500 text-white rounded" @click="isAddMovieOpen = !isAddMovieOpen">Add Movie</button>

  <FormAddMovie v-if="isAddMovieOpen" :movies :handleClose="handleClose" />
  <section class="flex flex-row flex-wrap gap-5 w-full pt-10 items-center justify-center">
    <div
      class="relative flex h-[32rem] flex-col items-start justify-between rounded overflow-hidden bg-white"
      v-for="movie in movies"
      :key="movie.id"
    >
      <aside class="absolute top-2 right-2">
        <StarIcon class="w-9 text-yellow-500" :class="{ 'text-slate-500': !movie.rating }" />
        <span class="font-sm absolute top-1/2 right-1/2 translate-x-1/2 -translate-y-1/2">{{ movie.rating ? movie.rating : "-" }}</span>
      </aside>

      <img class="w-64 max-h-80 object-cover" :src="movie.image" />
      <section class="px-5 flex flex-col justify-between pb-2 h-full w-full">
        <p class="font-bold pt-2">{{ movie.name }}</p>
        <div class="flex flex-row gap-1">
          <p class="text-xs bg-blue-500 rounded-full px-2 py-0.5 text-white" v-for="genre in movie.genres">{{ genre }}</p>
        </div>
        <p class="max-w-52 text-xs text-slate-800 mt-4">
          {{ movie.description }}
        </p>
        <p class="text-sm mt-4 flex items-center gap-2 flex-row">
          Rating ({{ movie.rating }}/5)
          <span class="flex flex-row items-center">
            <button v-for="n in 5" @click="() => handleStar(movie.id, n)" :disabled="n === movie.rating">
              <StarIcon class="w-4" :class="n <= movie.rating ? 'text-yellow-500' : 'text-slate-500'" />
            </button>
          </span>
        </p>
      </section>
    </div>
  </section>
</template>
