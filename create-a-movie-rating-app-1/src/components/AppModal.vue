<script setup>
const emit = defineEmits(["handleClose"]);
const props = defineProps(["title"]);
const close = () => {
  emit("handleClose");
};
</script>

<template>
  <div class="relative z-48 text-white p-52 w-screen h-screen top-0 left-0 backdrop-blur-xl flex justify-center items-center">
    <div class="bg-slate-800 relative p-10 w-full flex flex-col items-start justify-start gap-5 rounded">
      <p @click="close" class="absolute right-4 top-2 text-white cursor-pointer hover:text-red-500">X</p>
      <p v-if="title" class="text-white font-bold">{{ title }}</p>
      <slot />
    </div>
  </div>
</template>
