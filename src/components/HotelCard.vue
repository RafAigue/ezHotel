<script setup>
const props = defineProps({
  hotel: {
    type: Object,
    required: true,
    default: {},
  },
});

const emit = defineEmits(["handleHotelSelected"]);
</script>

<template>
  <div
    class="p-2 gap-1 flex flex-row bg-zinc-100/50 shadow-sm shadow-gray-400 rounded-xl transition duration-200 ease-in-out hover:bg-white"
  >
    <img
      :src="hotel?.image"
      class="w-[33%] rounded-xl aspect-video object-cover"
    />
    <div class="flex flex-col flex-1 justify-between pl-2">
      <div class="flex flex-col">
        <span
          v-on:click="emit('handleHotelSelected', hotel)"
          class="text-lg font-bold text-sky-600 pr-2 transition duration-200 ease-in-out hover:text-sky-800 hover:cursor-pointer"
          >{{ hotel?.name }}</span
        >
        <span class="text-sm text-yellow-700 font-light">{{
          hotel?.accommodation_type
        }}</span>
        <div class="flex flex-row gap-1">
          <span
            v-for="label in hotel?.merchandising_labels"
            class="p-1 bg-sky-200 rounded-md text-xs border-1 border-zinc-300"
            >{{ label }}</span
          >
        </div>
      </div>
      <div class="flex flex-col">
        <div class="flex flex-col text-xs pb-1">
          <span> Cheapest: {{ hotel?.price_ranges.minimum }}€ </span>
          <span> Premium: {{ hotel?.price_ranges.maximum }}€ </span>
        </div>
        <div class="flex flex-row gap-1">
          <span
            v-for="mention in hotel?.mentions"
            class="text-xs font-light bg-zinc-200 rounded-md p-1 border-1 border-zinc-300"
          >
            {{ mention }}
          </span>
        </div>
      </div>
    </div>
    <div class="flex flex-col w-[11%] justify-between">
      <div class="flex flex-row text-right">
        <span class="text-xs font-light text-zinc-700 pr-1.5">
          {{ hotel?.review_summary.count }} reviews
        </span>
        <span class="bg-sky-700 p-1 rounded-lg font-bold text-white">
          {{ hotel?.review_summary.rating.toFixed(1) }}
        </span>
      </div>
      <a
        :href="hotel?.url"
        target="_blank"
        class="p-1 text-white text-sm bg-sky-500 rounded-md hover:cursor-pointer"
        >Check hotel
      </a>
    </div>
  </div>
</template>
