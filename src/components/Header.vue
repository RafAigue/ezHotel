<script setup>
const props = defineProps({
  city: {
    type: String,
    required: true,
    default: "",
  },
});
</script>

<template>
  <header
    class="flex items-center bg-gradient-to-b from-blue-300 to-white h-20 px-5 py-2"
  >
    <img
      src="../assets/tripadvisor.png"
      class="fixed w-[100px] rounded-xl aspect-video object-cover"
    />
    <span class="text-4xl text-zinc-900 text-center font-bold flex-1">{{
      city
    }}</span>
  </header>
</template>
