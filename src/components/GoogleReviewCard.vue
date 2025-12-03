<template>
  <div class="review-card">
    <div class="review-card__body">
      <div class="review-card__header">
        <img src="/public/google.svg" alt="Google logo">
        <h2 class="review-card__title">{{ title }}</h2>
      </div>

      <div v-if="!isLoading && data" class="review-card__content">
        <div class="review-card__rating-row">
          <span class="rating-number">{{ data.rating }}</span>
          <StarRating :rating="data.rating" :size="24" />
        </div>
        <p class="review-card__count">{{ data.reviewsCount }} відгуки</p>
      </div>
      <div v-else class="review-card__loading">Завантаження...</div>
    </div>


    <div class="review-card__actions">
      <button @click="handleRedirect" class="btn btn--secondary">Переглянути</button>
      <button @click="handleModal" class="btn btn--primary">Написати</button>
    </div>

    <BaseModal
      v-model="isModalOpen"
      title="Написати відгук"
    >
      <div class="review-form">
        <p>Ми будемо вдячні за вашу думку про наш сервіс!</p>

        <textarea
          class="review-textarea"
          placeholder="Введіть ваш відгук тут..."
          rows="4"
        ></textarea>
      </div>

      <template #footer>
        <button class="btn btn--secondary" @click="isModalOpen = false">Скасувати</button>
        <button class="btn btn--primary" @click="isModalOpen = false">Надіслати</button>
      </template>
    </BaseModal>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import StarRating from "@/components/StarRating.vue";
import BaseModal from "@/components/ui/BaseModal.vue";

interface Props {
  apiUrl: string;
  redirectUrl?: string;
  title?: string;
}

const props = withDefaults(defineProps<Props>(), {
  redirectUrl: 'https://www.google.com/',
  title: 'Відгуки наших клієнтів у Google',
});

interface ReviewData {
  rating: number;
  reviewsCount: number;
}

const data = ref<ReviewData | null>(null);
const isLoading = ref(true);
const isModalOpen = ref(false);

const fetchData = async () => {
  try {
    isLoading.value = true;
    const response = await fetch(props.apiUrl);

    if (!response.ok) throw new Error('Network response was not ok');

    const result = await response.json();
    data.value = Array.isArray(result) ? result[0] : result;
  } catch (error) {
    console.error('Error fetching data:', error);
    data.value = { rating: 0, reviewsCount: 0 };
  } finally {
    isLoading.value = false;
  }
};

onMounted(() => {
  fetchData();
});

const handleRedirect = () => {
  window.open(props.redirectUrl, '_blank');
};

const handleModal = () => {
  isModalOpen.value = !isModalOpen.value;
};
</script>

<style scoped lang="scss">
$color-bg: #ffffff;
$color-text: #394155;
$color-text-light: #798595;
$shadow: 0 4px 15px rgba(0,0,0, 5%);
$radius: 16px;

.review-card {
  background: $color-bg;
  border-radius: $radius;
  box-shadow: $shadow;
  padding: 30px;
  width: 100%;
  box-sizing: border-box;
  text-align: left;
  margin: 0 auto;

  &__header {
    display: flex;
    align-items: center;
    gap: 16px;
    margin-bottom: 20px;
  }

  &__title {
    font-family: 'FormularMedium', 'Roboto', sans-serif;
    font-size: 18px;
    color: $color-text;
    line-height: 26px;
    margin: 0;
  }

  &__rating-row {
    display: flex;
    align-items: center;
    gap: 20px;
  }

  .rating-number {
    font-family: 'GreenwichMedium', 'Roboto', sans-serif;
    font-size: 24px;
    color: $color-text;
  }

  &__count {
    color: $color-text-light;
    font-family: 'Formular', 'Roboto', sans-serif;
    font-size: 14px;
    margin: 7px 0 20px 0;
  }

  &__actions {
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  &__loading {
    margin-bottom: 14px;
  }
}

.btn {
  width: 100%;
  padding: 12px 24px;
  border-radius: 8px;
  font-family: 'FormularMedium', 'Roboto', sans-serif;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  border: none;
  transition: all 0.2s ease;

  &--secondary {
    border: 1px solid transparent;
    background:
        linear-gradient(90deg, #E6F7F5 0%, #F8FCFC) padding-box,
        linear-gradient(90deg, #AACEDB, #BEE2E0) border-box;
    color: $color-text;
    &:hover {
      background:
          linear-gradient(90deg, #E6F7F5 0%, #E6F7F5) padding-box,
          linear-gradient(90deg, #AACEDB, #BEE2E0) border-box;
    }
  }

  &--primary {
    background: linear-gradient(90deg, #3CB9A0 0%, #1786AC 100%);
    color: white;
    &:hover { opacity: 0.9; }
  }
}

.review-textarea {
  width: 100%;
  box-sizing: border-box;
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 8px;
  resize: vertical;
  margin-top: 10px;
  font-family: 'Formular', 'Roboto', sans-serif;

  &:focus {
    outline: none;
    border-color: #1e8e8e;
  }
}

@media (min-width: 577px) {
  .review-card {
    display:  flex;
    justify-content:  space-between;
    align-items: center;

    &__header {
      margin-right: 20px;
      margin-bottom: 10px;
    }

    &__content {
      display: flex;
      align-items: center;
    }

    &__count {
      margin: 0 0 0 20px;
    }
  }
}

@media (min-width: 760px) {
  .review-card {
    &__actions {
      flex-direction: row;
      height: max-content;
    }
  }
}

@media (min-width: 1200px) {
  .review-card {
    &__body {
      display: flex;
      gap: 50px;
    }

    &__header {
      margin: 0;
    }

    &__title {
      font-size: 20px;
    }

    .rating-number {
      font-size: 36px;
    }
  }
}

@media (min-width: 1367px) {
  .review-card {
    &__body {
      gap: 60px;
    }
  }
}
</style>