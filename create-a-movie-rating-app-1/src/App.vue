<script setup>
import { reactive, ref } from "vue";
import { items } from "./movies.json";
import { StarIcon } from "@heroicons/vue/24/solid";

const movies = reactive(items);
console.log(movies);
</script>

<template>
  <section class="flex flex-row flex-wrap gap-5 items-center p-5 justify-center">
    <div class="flex flex-col items-start justify-between rounded overflow-hidden bg-white pb-5" v-for="movie in movies" :key="movie.id">
      <img class="w-64 max-h-80 object-cover" :src="movie.image" />
      <section class="px-5">
        <p class="font-bold">{{ movie.name }}</p>
        <div class="flex flex-row gap-1">
          <p class="text-xs bg-blue-500 rounded-full px-2 py-0.5 text-white" v-for="genre in movie.genres">{{ genre }}</p>
        </div>
        <p class="max-w-52 text-xs text-slate-800 mt-4">
          {{ movie.description }}
        </p>
        <p class="text-sm mt-4 flex items-center gap-2 flex-row">
          Rating ({{ movie.rating }}/5)
          <span class="flex flex-row items-center"><StarIcon v-for="n in movie.rating" class="text-yellow-500 w-4" /></span>
        </p>
      </section>
    </div>
  </section>
</template>
