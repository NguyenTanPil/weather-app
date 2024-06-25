<template>
  <Suspense>
    <AsyncCityView />

    <template #fallback> Loading... </template>
  </Suspense>
</template>

<script setup>
import AsyncCityView from '@/components/AsyncCityView.vue';
</script>
