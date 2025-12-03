<template>
  <div class="wrapper">
    <GoogleReviewCard
      :api-url="API_URL"
      redirect-url="https://www.google.com/maps"
    />
  </div>
</template>

<script setup lang="ts">
import GoogleReviewCard from '../components/GoogleReviewCard.vue';

const API_URL = 'https://69301be8778bbf9e006fdc05.mockapi.io/google-rating';
</script>

<style scoped>
.wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  padding: 20px;
}
</style>
