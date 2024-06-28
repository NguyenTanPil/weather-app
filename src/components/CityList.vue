<template>
  <p v-if="savedCities.length === 0">
    No locations added. To start tracking a location, search in the field above
  </p>
  <template v-if="isLoadedCities">
    <div v-for="city in savedCities" :key="city.id">
      <CityCard :city="city" @click="goToCityView(city)" />
    </div>
  </template>
</template>

<script setup>
import axios from 'axios';
import { onBeforeMount } from 'vue';
import { ref } from 'vue';
import CityCard from './CityCard.vue';
import { useRouter } from 'vue-router';

const router = useRouter();

const savedCities = ref([]);
const isLoadedCities = ref(false);

const getCities = async () => {
  const savedCitiesLocalStorage = localStorage.getItem('savedCities');
  if (savedCitiesLocalStorage) {
    savedCities.value = JSON.parse(savedCitiesLocalStorage);
  }

  const requests = [];
  savedCities.value.forEach((city) => {
    requests.push(
      axios.get(
        `https://api.openweathermap.org/data/2.5/weather?lat=${city.coords.lat}&lon=${city.coords.lng}&appid=7efa332cf48aeb9d2d391a51027f1a71&units=imperial`
      )
    );
  });

  const weatherData = await Promise.all(requests);
  weatherData.forEach((item, idx) => {
    savedCities.value[idx].weather = item.data;
  });

  isLoadedCities.value = true;
};

const goToCityView = (city) => {
  router.push({
    name: 'city',
    params: { state: city.state, city: city.city },
    query: { lat: city.coords.lat, lng: city.coords.lng }
  });
};

onBeforeMount(() => {
  getCities();
});
</script>
