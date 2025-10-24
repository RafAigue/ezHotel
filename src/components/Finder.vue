<script setup>
import { ref } from "vue";

const props = defineProps({
  cities: {
    type: Array,
    required: true,
    default: [],
  },
});

const sortedCities = ref([]);
const showCities = ref(false);

const emit = defineEmits(["changeCity"]);

const handleInputChange = (e) => {
  showCities.value = false;
  if (e.length < 3) return;
  let sorted = props.cities.filter((city) =>
    city.name.toLowerCase().includes(e.toLowerCase())
  );
  if (sorted.length > 0) {
    sortedCities.value = sorted;
    showCities.value = true;
  }
};
</script>

<template>
  <div class="p-2 flex justify-center h-25">
    <div class="flex flex-col">
      <div class="flex flex-row gap-2 items-center">
        <label for="city" class="block font-light">City</label>
        <div class="mt-1">
          <div
            class="flex items-center rounded-md pl-3 outline-1 -outline-offset-1 outline-gray-600 has-[input:focus-within]:outline-2 has-[input:focus-within]:-outline-offset-2 has-[input:focus-within]:outline-indigo-500"
          >
            <input
              id="city"
              type="text"
              name="coty"
              placeholder="Search city"
              class="block min-w-0 grow py-1.5 pr-3 pl-1 text-base placeholder:text-gray-500 focus:outline-none sm:text-sm"
              v-on:input="(e) => handleInputChange(e.target.value)"
            />
          </div>
        </div>
      </div>
      <div class="flex gap-2 p-2">
        <button
          v-if="showCities"
          v-for="city in sortedCities"
          v-on:click="emit('changeCity', city)"
          class="p-1 bg-sky-300 rounded-md hover:cursor-pointer transition duration-200 ease-in-out hover:bg-white"
        >
          {{ city.name }}
        </button>
      </div>
    </div>
  </div>
</template>
