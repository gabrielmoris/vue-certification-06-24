<script setup>
import { onMounted, ref } from "vue";
import AppModal from "./AppModal.vue";

const props = defineProps(["movies", "handleClose", "movie"]);
const nameInputRef = ref(null);

let movieToSend;
if (!props.movie) {
  movieToSend = ref({
    id: props.movies.length + 1,
    name: "",
    description: "",
    image: "",
    rating: 0,
    genres: [],
    inTheaters: false,
  });
} else {
  movieToSend = ref(props.movie);
}

const handleSubmit = (e) => {
  e.preventDefault();
  if (!movieToSend.value.name | !movieToSend.value.description | !movieToSend.value.image | movieToSend.value.genre) {
    return;
  }
  if (!props.movie) {
    props.movies.push(movieToSend.value);
  }

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

onMounted(() => nameInputRef.value.focus());
</script>

<template>
  <AppModal @handleClose="handleClose">
    <form class="w-full h-full" @submit="handleSubmit">
      <div class="flex flex-col w-full">
        <label>Name</label>
        <input ref="nameInputRef" class="border border-slate-500 p-1 rounded w-full bg-slate-900" type="text" v-model="movieToSend.name" />
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
  </AppModal>
</template>
