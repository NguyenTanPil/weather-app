<template>
  <div class="flex py-6 px-3 bg-weather-secondary rounded-md shadow-md"></div>
  <div class="flex flex-col flex-1 gap-2">
    <AnimatedPlaceholder class="max-w-[50%]" />
    <AnimatedPlaceholder class="max-w-[40%]" />

    <div class="flex flex-col items-end flex-1 gap-2">
      <AnimatedPlaceholder class="max-w-[50%]" />
      <AnimatedPlaceholder class="max-w-[75px] w-full" />
    </div>
  </div>
</template>

<script setup>
import AnimatedPlaceholder from './AnimatedPlaceholder.vue';
</script>
