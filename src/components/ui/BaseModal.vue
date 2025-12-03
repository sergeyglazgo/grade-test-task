<template>
  <Teleport to="body">
    <Transition name="modal-fade">
      <div
        v-if="modelValue"
        class="modal"
        @click="close"
      >
        <div
          class="modal__container"
          :style="{ maxWidth: width }"
          @click.stop
        >
          <div class="modal__header">
            <h3 v-if="title" class="modal__title">{{ title }}</h3>
            <button class="modal__close-btn" @click="close">
              <svg viewBox="0 0 24 24" width="24" height="24" fill="none" stroke="currentColor" stroke-width="2">
                <line x1="18" y1="6" x2="6" y2="18"></line>
                <line x1="6" y1="6" x2="18" y2="18"></line>
              </svg>
            </button>
          </div>

          <div class="modal__content">
            <slot></slot>
          </div>

          <div v-if="$slots.footer" class="modal__footer">
            <slot name="footer"></slot>
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup lang="ts">
interface Props {
  modelValue: boolean;
  title?: string;
  width?: string;
}

withDefaults(defineProps<Props>(), {
  modelValue: false,
  title: '',
  width: '500px'
});

const emit = defineEmits<{
  (e: 'update:modelValue', value: boolean): void;
}>();

const close = () => {
  emit('update:modelValue', false);
};
</script>

<style scoped lang="scss">
$modal-overlay-bg: rgba(0, 0, 0, 0.5);
$modal-bg: #ffffff;
$radius: 12px;
$z-index: 1000;

.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: $modal-overlay-bg;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: $z-index;
  padding: 20px;
  font-family: 'FormularMedium', 'Roboto', sans-serif;

  &__container {
    background: $modal-bg;
    border-radius: $radius;
    width: min(var(--modal-width, 600px), 100%);
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
    display: flex;
    flex-direction: column;
    position: relative;
    max-height: 90vh;
    overflow: hidden;
  }

  &__header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20px 24px;
    border-bottom: 1px solid #eee;
  }

  &__title {
    margin: 0;
    font-size: 1.25rem;
    font-weight: 600;
    color: #333;
  }

  &__close-btn {
    background: none;
    border: none;
    cursor: pointer;
    color: #999;
    padding: 4px;
    display: flex;
    align-items: center;
    border-radius: 4px;
    transition: color 0.2s, background 0.2s;

    &:hover {
      color: #333;
      background: #f5f5f5;
    }
  }

  &__content {
    padding: 24px;
    overflow-y: auto;
  }

  &__footer {
    padding: 16px 24px;
    border-top: 1px solid #eee;
    display: flex;
    justify-content: flex-end;
    gap: 12px;
  }
}

.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.3s ease;

  .modal__container {
    transition: transform 0.3s ease;
  }
}

.modal-fade-enter-from,
.modal-fade-leave-to {
  opacity: 0;

  .modal__container {
    transform: scale(0.95);
  }
}
</style>