<script setup>
const props = defineProps({
  accomodationFiltersOptions: {
    type: Array,
    required: false,
    default: [],
  },
  merchandisingFiltersOptions: {
    type: Array,
    required: false,
    default: [],
  },
  accomodationFilters: {
    type: Array,
    required: false,
    default: [],
  },
  merchandisingFilters: {
    type: Array,
    required: false,
    default: [],
  },
});

const emit = defineEmits(["handleFilterChecked"]);
</script>

<template>
  <div class="flex flex-col p-2 w-[263px] gap-5">
    <div v-if="accomodationFiltersOptions.length > 0" class="flex flex-col p-1">
      <span class="font-bold text-xl">Accommodation</span>
      <div
        v-for="option in accomodationFiltersOptions"
        :key="option"
        class="p-1 text-sm"
      >
        <input
          type="checkbox"
          :id="`accommodation-${option}`"
          :name="`accommodation-${option}`"
          :checked="accomodationFilters.includes(option)"
          v-on:click="emit('handleFilterChecked', 'accommodation', option)"
        />
        <label class="pl-1.5" :for="`accommodation-${option}`">{{
          option
        }}</label>
      </div>
    </div>
    <div
      v-if="merchandisingFiltersOptions.length > 0"
      class="flex flex-col p-1"
    >
      <span class="font-bold text-xl">Merchandising</span>
      <div
        v-for="option in merchandisingFiltersOptions"
        :key="option"
        class="p-1 text-sm"
      >
        <input
          type="checkbox"
          :id="`merchandising-${option}`"
          :name="`merchandising-${option}`"
          :checked="merchandisingFilters.includes(option)"
          v-on:click="emit('handleFilterChecked', 'merchandising', option)"
        />
        <label class="pl-1.5" :for="`merchandising-${option}`">{{
          option
        }}</label>
      </div>
    </div>
  </div>
</template>
