<script setup>
import { onMounted, ref } from "vue";
import VueDatePicker from "@vuepic/vue-datepicker";
import "@vuepic/vue-datepicker/dist/main.css";

const props = defineProps({
  hotel: {
    type: Object,
    required: true,
    default: {},
  },
});

let today = new Date();
let twoDays = new Date();
const date = ref();

twoDays.setDate(twoDays.getDate() + 2);

const formatDate = (date) => date.toISOString().split("T")[0];

const fromDate = ref(formatDate(today));
const toDate = ref(formatDate(twoDays));

const emit = defineEmits(["handleHotelSelected"]);

const fetchData = async () => {
  const resp = await fetch(getCityUrl());
  let jsonData = await resp.json();
  data.value = jsonData.result;
  getFilters();
};

onMounted(async () => {
  try {
    const startDate = new Date();
    const endDate = new Date(new Date().setDate(startDate.getDate() + 7));
    date.value = [startDate, endDate];
    await fetchData();
    fetching.value = false;
  } catch (error) {
    console.error("Error fetching data:", error);
  }
});
</script>

<template>
  <div
    className="fixed inset-0 bg-black/10 backdrop-blur-xs flex items-center justify-center z-1"
  >
    <div
      className="bg-white p-4 rounded-xl shadow-lg w-300 border border-sky-300"
    >
      <div class="flex justify-between">
        <span class="font-bold text-xl text-sky-800">{{
          props.hotel.name
        }}</span>
        <span
          class="font-bold hover:cursor-pointer"
          v-on:click="emit('handleHotelSelected', null)"
          >X</span
        >
      </div>
      <div className="grid grid-cols-2 justify-between pt-5 gap-5">
        <img
          :src="props.hotel?.image"
          class="rounded-xl aspect-video object-cover"
        />
        <div class="flex flex-col p-5 w-full h-full bg-zinc-100">
          <span>Check-in: {{ fromDate }}</span>
          <span>Check-out: {{ toDate }}</span>
          <VueDatePicker v-model="date" range />
        </div>
      </div>
    </div>
  </div>
</template>
