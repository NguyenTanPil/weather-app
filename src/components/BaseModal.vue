<template>
  <Teleport to="body">
    <Transition name="modal-outer">
      <div
        class="absolute w-full bg-black bg-opacity-30 h-screen top-0 left-0 flex justify-center px-8"
        v-show="modalActive"
      >
        <Transition name="modal-inner">
          <div class="p-4 bg-white self-start mt-32 max-w-screen-md" v-if="modalActive">
            <slot />

            <button
              class="text-white mt-8 bg-weather-primary py-2 px-6"
              @click="emit('closeModal')"
            >
              Close
            </button>
          </div>
        </Transition>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup>
defineProps({
  modalActive: {
    type: Boolean,
    default: false
  }
});

const emit = defineEmits(['close-modal']);
</script>

<style scoped>
.modal-outer-enter-active,
.modal-outer-leave-active {
  transition: opacity 0.3s cubic-bezier(0.075, 0.82, 0.165, 1);
}

.modal-outer-enter-from,
.modal-outer-leave-to {
  opacity: 0;
}

.modal-inner-enter-active {
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275) 0.15s;
}

.modal-inner-enter-active {
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.modal-inner-enter-from {
  opacity: 0;
  transform: scale(0.8);
}

.modal-inner-leave-to {
  transform: scale(0.8);
}
</style>
