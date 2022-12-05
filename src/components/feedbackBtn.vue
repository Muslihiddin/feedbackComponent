<script setup>
import { ref } from "vue";
import feedbackModal from "./feedbackModal.vue";

let isModalOpen = ref(false);
const openModal = () => {
  isModalOpen.value = !isModalOpen.value;
};
</script>
<template>
  <div class="relative">
    <button @click="openModal">Feedback</button>
    <feedbackModal
      :isModalOpen="isModalOpen"
      @close-modal="isModalOpen = false"
    />
  </div>
</template>
