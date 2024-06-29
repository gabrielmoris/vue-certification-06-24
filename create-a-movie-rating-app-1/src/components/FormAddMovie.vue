<script setup>
import { ref } from "vue";

const props = defineProps(["movies", "handleClose"]);

const movieToSend = ref({
  id: props.movies.length + 1,
  name: "",
  description: "",
  image: "",
  rating: 0,
  genres: [],
  inTheaters: false,
});

const handleSubmit = (e) => {
  e.preventDefault();
  if (!movieToSend.value.name | !movieToSend.value.description | !movieToSend.value.image | movieToSend.value.genre) {
    return;
  }
  props.movies.push(movieToSend.value);
  movieToSend.value = {
    id: props.movies.length,
    name: "",
    description: "",
    image: "",
    rating: 0,
    genres: [],
    inTheaters: false,
  };
  props.handleClose();
};

const handleClear = () => {
  movieToSend.value = {
    id: props.movies.length,
    name: "",
    description: "",
    image: "",
    rating: 0,
    genres: [],
    inTheaters: false,
  };
};
</script>

<template>
  <div class="absolute z-50 text-white p-52 w-screen h-screen top-0 left-0 backdrop-blur-xl flex justify-center items-center">
    <form class="bg-slate-800 p-10 w-full flex flex-col items-start justify-start gap-5 rounded" @submit="handleSubmit">
      <div class="flex flex-col w-full">
        <label>Name</label>
        <input class="border border-slate-500 p-1 rounded w-full bg-slate-900" type="text" v-model="movieToSend.name" />
      </div>
      <div class="flex flex-col w-full">
        <label>Description</label>
        <input required class="border border-slate-500 p-1 rounded w-full bg-slate-900" type="text" v-model="movieToSend.description" />
      </div>
      <div class="flex flex-col w-full">
        <label>Image</label>
        <input required class="border border-slate-500 p-1 rounded w-full bg-slate-900" type="text" v-model="movieToSend.image" />
      </div>
      <div class="flex flex-col w-full">
        <label>Genres</label>
        <select required class="text-slate-900" v-model="movieToSend.genres" multiple>
          <option>Drama</option>
          <option>Comedy</option>
          <option>Action</option>
          <option>Terror</option>
          <option>Crime</option>
        </select>
      </div>
      <div>
        <input class="mr-5" type="checkbox" v-bind="movieToSend.inTheaters" />
        <label>In Theaters</label>
      </div>
      <div class="w-full flex flex-row justify-between">
        <button @click.prevent="handleClear" class="px-3 py-1 bg-slate-600 text-black rounded">Clear</button>
        <button @click="handleSubmit" class="px-3 py-1 bg-blue-500 text-white rounded" type="submit">Submit</button>
      </div>
    </form>
  </div>
</template>
