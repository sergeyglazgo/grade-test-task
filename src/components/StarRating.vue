<template>
  <div class="star-rating">
    <div
      v-for="index in maxStars"
      :key="index"
      class="star-rating__item"
      :style="{ width: `${size}px`, height: `${size}px` }"
    >
      <svg
        class="star-rating__icon star-rating__icon--bg"
        viewBox="0 0 25 25"
      >
        <path d="M12 .587l3.668 7.568L24 9.748l-6 5.848L19.335 24 12 19.771 4.665 24 6 15.596 0 9.748l8.332-1.593z" />
      </svg>

      <svg
        class="star-rating__icon star-rating__icon--filled"
        viewBox="0 0 25 25"
        :style="{ clipPath: `inset(0 ${100 - getFillPercent(index)}% 0 0)` }"
      >
        <path d="M12 .587l3.668 7.568L24 9.748l-6 5.848L19.335 24 12 19.771 4.665 24 6 15.596 0 9.748l8.332-1.593z" />
      </svg>
    </div>
  </div>
</template>

<script setup lang="ts">
interface Props {
  rating: number;
  maxStars?: number;
  size?: number;
}

const props = withDefaults(defineProps<Props>(), {
  maxStars: 5,
  size: 24
});

const getFillPercent = (index: number): number => {
  const value = props.rating - (index - 1);
  if (value >= 1) return 100;
  if (value <= 0) return 0;
  return value * 100;
};
</script>

<style scoped lang="scss">
.star-rating {
  display: flex;
  gap: 4px;
  align-items: center;

  &__item {
    position: relative;
    display: block;
  }

  &__icon {
    width: 100%;
    height: 100%;
    display: block;

    &--bg {
      fill: #ffffff;
      stroke: #FCC141;
      stroke-width: 2px;
      stroke-linejoin: round;
    }

    &--filled {
      fill: #FCC141;
      stroke: none;
      position: absolute;
      top: 0;
      left: 0;
      transition: clip-path 0.3s ease-out;
    }
  }
}
</style>