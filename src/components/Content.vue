<script setup>
import { ref, computed } from "vue";
import HotelCard from "./HotelCard.vue";
import HotelModal from "./HotelModal.vue";

const props = defineProps({
  hotels: {
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

const sortBy = ref(null);
const hotelSelected = ref(null);

const buttonClasses =
  "mx-1 p-1 bg-zinc-200 rounded-md border-1 border-zinc-300 hover:cursor-pointer transition duration-200 ease-in-out hover:bg-white";

const filteredHotels = computed(() => {
  console.log("filteredHotelsfilteredHotelsfilteredHotels");
  let filtered = [...props.hotels];

  if (props.accomodationFilters.length > 0) {
    filtered = filtered.filter((hotel) =>
      props.accomodationFilters.includes(hotel.accommodation_type)
    );
  }

  if (props.merchandisingFilters.length > 0) {
    filtered = filtered.filter((hotel) =>
      hotel.merchandising_labels.some((label) =>
        props.merchandisingFilters.includes(label)
      )
    );
  }

  if (sortBy.value) {
    filtered = [...filtered].sort((a, b) => {
      switch (sortBy.value) {
        case "cheapest":
          return a.price_ranges.minimum - b.price_ranges.minimum;
        case "premium":
          return a.price_ranges.maximum - b.price_ranges.maximum;
        case "rate":
          return b.review_summary.rating - a.review_summary.rating;
        case "review":
          return b.review_summary.count - a.review_summary.count;
        default:
          return 0;
      }
    });
  }

  return filtered;
});

const handleSort = (sortType) => {
  sortBy.value = sortType;
};

const handleHotelSelected = (hotel) => {
  hotelSelected.value = hotel;
};
</script>

<template>
  <div class="px-4 flex-1 flex flex-col gap-3">
    <div class="relative">
      <div class="absolute top-[-25px] right-0 text-sm">
        <span class="pr-1">Sort by:</span>
        <button :class="buttonClasses" v-on:click="handleSort('cheapest')">
          Cheapest price (lowest first)
        </button>
        <button :class="buttonClasses" v-on:click="handleSort('premium')">
          Premium price (lowest first)
        </button>
        <button :class="buttonClasses" v-on:click="handleSort('rate')">
          Rate (highest first)
        </button>
        <button :class="buttonClasses" v-on:click="handleSort('review')">
          Reviews (top first)
        </button>
      </div>
    </div>
    <HotelCard
      v-for="hotel in filteredHotels"
      :key="hotel.key"
      :hotel="hotel"
      @handleHotelSelected="handleHotelSelected"
    />
  </div>
  <HotelModal
    v-if="hotelSelected !== null"
    :hotel="hotelSelected"
    @handleHotelSelected="handleHotelSelected"
  />
</template>
