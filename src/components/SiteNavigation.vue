<template>
  <header class="sticky top-0 bg-weather-primary shadow-lg">
    <nav class="container flex flex-col sm:flex-row items-center gap-4 text-white py-6">
      <router-link :to="{ name: 'home' }">
        <div class="flex items-center gap-3">
          <i class="fa-solid fa-sun text-2xl"></i>
          <p class="text-2xl">The Local Weather</p>
        </div>
      </router-link>

      <div class="flex gap-3 flex-1 justify-end">
        <i
          class="fa-solid fa-circle-info text-xl hover:text-weather-secondary duration-150 cursor-pointer"
          @click="toggleModal"
        ></i>

        <i
          class="fa-solid fa-plus text-xl hover:text-weather-secondary duration-150 cursor-pointer"
          @click="addCity"
          v-if="route.query.preview"
        ></i>
      </div>

      <BaseModal :modal-active="isShowModal" @close-modal="toggleModal">
        <div class="text-black">hello</div>
      </BaseModal>
    </nav>
  </header>
</template>

<script setup>
import BaseModal from '@/components/BaseModal.vue';
import { ref } from 'vue';
import { uid } from 'uid';
import { useRoute, useRouter } from 'vue-router';

const route = useRoute();
const router = useRouter();

const isShowModal = ref(false);
const savedCities = ref([]);

const toggleModal = () => {
  isShowModal.value = !isShowModal.value;
};

const addCity = () => {
  const savedCitiesLocalStorage = localStorage.getItem('savedCities');
  if (savedCitiesLocalStorage) {
    savedCities.value = JSON.parse(savedCitiesLocalStorage);
  }

  const locationObject = {
    id: uid(),
    state: route.params.state,
    city: route.params.city,
    coords: {
      lat: route.query.lat,
      lng: route.query.lng
    }
  };

  savedCities.value.push(locationObject);
  localStorage.setItem('savedCities', JSON.stringify(savedCities.value));

  let query = { ...route.query };
  delete query.preview;
  router.replace({ query });
};
</script>
