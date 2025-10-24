<script setup>
const citiesArr = [
  { name: "New York", id: "g60763" },
  { name: "London", id: "g186338" },
  { name: "Paris", id: "g187147" },
  { name: "Tokyo", id: "g298184" },
  { name: "Rome", id: "g187791" },
  { name: "Barcelona", id: "g187497" },
  { name: "Dubai", id: "g295424" },
  { name: "Singapore", id: "g294265" },
  { name: "Bangkok", id: "g293916" },
  { name: "Hong Kong", id: "g294217" },
  { name: "Istanbul", id: "g293974" },
  { name: "Madrid", id: "g187514" },
  { name: "Dublin", id: "g186605" },
  { name: "Lisbon", id: "g189158" },
  { name: "Berlin", id: "g187323" },
  { name: "Vienna", id: "g190454" },
  { name: "Amsterdam", id: "g188590" },
  { name: "Prague", id: "g274707" },
  { name: "Budapest", id: "g274887" },
  { name: "Copenhagen", id: "g189541" },
  { name: "Stockholm", id: "g189852" },
  { name: "Oslo", id: "g190479" },
  { name: "Helsinki", id: "g189934" },
  { name: "Brussels", id: "g188644" },
  { name: "Zurich", id: "g188113" },
  { name: "Geneva", id: "g188057" },
  { name: "Milan", id: "g187849" },
  { name: "Florence", id: "g187895" },
  { name: "Venice", id: "g187870" },
  { name: "Naples", id: "g187785" },
  { name: "Munich", id: "g187309" },
  { name: "Frankfurt", id: "g187337" },
  { name: "Hamburg", id: "g187331" },
  { name: "Cologne", id: "g187371" },
  { name: "Warsaw", id: "g274856" },
  { name: "Krakow", id: "g274772" },
  { name: "Athens", id: "g189400" },
  { name: "Edinburgh", id: "g186525" },
  { name: "Glasgow", id: "g186534" },
  { name: "Manchester", id: "g187069" },
  { name: "Birmingham", id: "g186402" },
  { name: "Liverpool", id: "g186337" },
  { name: "Leeds", id: "g186411" },
  { name: "Cardiff", id: "g186460" },
  { name: "Belfast", id: "g186470" },
  { name: "Cork", id: "g186600" },
  { name: "Galway", id: "g186609" },
  { name: "Valencia", id: "g187529" },
  { name: "Seville", id: "g187443" },
  { name: "Malaga", id: "g187438" },
  { name: "Granada", id: "g187441" },
  { name: "Bilbao", id: "g187454" },
  { name: "Porto", id: "g189180" },
  { name: "Bruges", id: "g188671" },
  { name: "Nice", id: "g187234" },
  { name: "Lyon", id: "g187265" },
  { name: "Marseille", id: "g187253" },
  { name: "Toulouse", id: "g187175" },
  { name: "Bordeaux", id: "g187079" },
  { name: "Moscow", id: "g298484" },
  { name: "Saint Petersburg", id: "g298507" },
  { name: "Kyiv", id: "g294474" },
  { name: "Toronto", id: "g155019" },
  { name: "Vancouver", id: "g154943" },
  { name: "Montreal", id: "g155032" },
  { name: "Quebec City", id: "g155033" },
  { name: "Chicago", id: "g35805" },
  { name: "Los Angeles", id: "g32655" },
  { name: "San Francisco", id: "g60713" },
  { name: "Las Vegas", id: "g45963" },
  { name: "Miami", id: "g34438" },
  { name: "Boston", id: "g60745" },
  { name: "Washington DC", id: "g28970" },
  { name: "Seattle", id: "g60878" },
  { name: "Austin", id: "g30196" },
  { name: "Dallas", id: "g55711" },
  { name: "Houston", id: "g56003" },
  { name: "Atlanta", id: "g60898" },
  { name: "San Diego", id: "g60750" },
  { name: "Philadelphia", id: "g60795" },
  { name: "Denver", id: "g33388" },
  { name: "Phoenix", id: "g31310" },
  { name: "Mexico City", id: "g150800" },
  { name: "Cancun", id: "g150807" },
  { name: "Buenos Aires", id: "g312741" },
  { name: "Santiago", id: "g294305" },
  { name: "Rio de Janeiro", id: "g303506" },
  { name: "São Paulo", id: "g303631" },
  { name: "Lima", id: "g294316" },
  { name: "Bogota", id: "g294074" },
  { name: "Caracas", id: "g316069" },
  { name: "Cape Town", id: "g312659" },
  { name: "Johannesburg", id: "g312578" },
  { name: "Cairo", id: "g294201" },
  { name: "Marrakech", id: "g293734" },
  { name: "Casablanca", id: "g293732" },
  { name: "Nairobi", id: "g294207" },
  { name: "Delhi", id: "g304551" },
  { name: "Mumbai", id: "g304554" },
  { name: "Bangalore", id: "g297628" },
  { name: "Beijing", id: "g294212" },
  { name: "Shanghai", id: "g308272" },
  { name: "Seoul", id: "g294197" },
  { name: "Taipei", id: "g293913" },
  { name: "Sydney", id: "g255060" },
  { name: "Melbourne", id: "g255100" },
  { name: "Auckland", id: "g255106" },
  { name: "Wellington", id: "g255115" },
];
import { ref, onMounted } from "vue";
import Header from "./components/Header.vue";
import Sidebar from "./components/Sidebar.vue";
import Content from "./components/Content.vue";
import Finder from "./components/Finder.vue";

const data = ref(null);
const city = ref(citiesArr[0]);
const accomodationFilters = ref([]);
const merchandisingFilters = ref([]);
const accomodationFiltersOptions = ref([]);
const merchandisingFiltersOptions = ref([]);
const fetching = ref(true);

const fetchData = async () => {
  const resp = await fetch(getCityUrl());
  let jsonData = await resp.json();
  data.value = jsonData.result;
  getFilters();
};

const getCityUrl = () => {
  return `https://data.xotelo.com/api/list?location_key=${city.value.id}&offset=0&limit=10&sort=best_value`;
};

const changeCity = async (newCity) => {
  fetching.value = true;
  city.value = newCity;
  await fetchData();
  fetching.value = false;
};

const getFilters = () => {
  let accommodation_type = [];
  let merchandising_labels = [];
  data.value.list.forEach((elem) => {
    !accommodation_type.includes(elem.accommodation_type) &&
      accommodation_type.push(elem.accommodation_type);
    elem.merchandising_labels.forEach((label) => {
      !merchandising_labels.includes(label) && merchandising_labels.push(label);
    });
  });

  accomodationFiltersOptions.value = accommodation_type;
  merchandisingFiltersOptions.value = merchandising_labels;

  console.log(
    "getFilters",
    accomodationFiltersOptions.value,
    merchandisingFiltersOptions.value
  );
};

const handleFilterChecked = (filterType, filterName) => {
  if (filterType === "accommodation") {
    if (accomodationFilters.value.includes(filterName)) {
      accomodationFilters.value = accomodationFilters.value.filter(
        (item) => item !== filterName
      );
    } else {
      accomodationFilters.value.push(filterName);
    }
  } else if (filterType === "merchandising") {
    if (merchandisingFilters.value.includes(filterName)) {
      merchandisingFilters.value = merchandisingFilters.value.filter(
        (item) => item !== filterName
      );
    } else {
      merchandisingFilters.value.push(filterName);
    }
  }
  console.log(
    "handleFilterChecked",
    accomodationFilters.value,
    merchandisingFilters.value
  );
};

onMounted(async () => {
  try {
    await fetchData();
    fetching.value = false;
  } catch (error) {
    console.error("Error fetching data:", error);
  }
});
</script>

<template>
  <div class="mb-5 flex flex-col font-sans">
    <Header :city="city.name" />
    <Finder :cities="citiesArr" @changeCity="changeCity" />
    <div class="flex flex-row w-280 m-auto">
      <Sidebar
        :accomodationFiltersOptions="accomodationFiltersOptions"
        :merchandisingFiltersOptions="merchandisingFiltersOptions"
        :accomodationFilters="accomodationFilters"
        :merchandisingFilters="merchandisingFilters"
        @handleFilterChecked="handleFilterChecked"
      />
      <Content
        v-if="data !== null && fetching == false"
        :hotels="data.list"
        :accomodationFilters="accomodationFilters"
        :merchandisingFilters="merchandisingFilters"
      />
      <div v-if="fetching == true" class="flex justify-center w-full pt-10">
        <span class="text-3xl font-bold text-sky-600">Loading...</span>
      </div>
    </div>
  </div>
</template>
