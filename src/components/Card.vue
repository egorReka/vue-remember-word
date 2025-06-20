<script setup>
import True from '../icons/True.vue';
import False from '../icons/False.vue';
const { word, translation, state, status } = defineProps({
  word: String,
  translation: String,
  state: String,
  status: String,
});

const emit = defineEmits(['isRemembered', 'flip']);
</script>

<template>
  <div
    class="card"
    :class="{ 'card--flipped': state === 'open' }"
    @click="emit('flip')"
  >
    <div class="card__front">
      <div class="card__container">
        <p class="card__word">{{ word }}</p>
        <div class="card__buttons">
          <button class="card__button" @click="emit('flip')">
            перевернуть
          </button>
        </div>
      </div>
    </div>

    <div class="card__back">
      <div class="card__container">
        <span class="card__status">
          <True v-if="status === 'success'" class="card__status-icon" />
          <False v-else-if="status === 'fail'" class="card__status-icon" />
        </span>

        <p class="card__translation">{{ translation }}</p>

        <div class="card__buttons">
          <template v-if="status === 'pending'">
            <button
              class="card__button"
              aria-label="не запомнил"
              @click="emit('isRemembered', false)"
            >
              <False class="card__icon" />
            </button>
            <button
              class="card__button"
              aria-label="запомнил"
              @click="emit('isRemembered', true)"
            >
              <True class="card__icon" />
            </button>
          </template>
          <span v-else class="card__status-completed"> ЗАВЕРШЕНО </span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.card {
  position: relative;
  min-height: 376px;
  font-size: 18px;
  text-align: center;
  color: var(--color-primary-inverted);
  background-color: var(--color-primary);
  border-radius: 16px;
  box-shadow: 0 0 16px 0 rgba(0, 0, 0, 0.1);
  transform-style: preserve-3d;
  transition: transform 0.5s;
}

.card.card--flipped {
  transform: rotateY(180deg);
}

.card.card--flipped .card__front .card__button {
  pointer-events: none;
}

.card.card--flipped .card__back {
  z-index: 3;
}

.card__container {
  position: relative;
  display: grid;
  place-content: center;
  width: 100%;
  height: 100%;
  padding: 16px;
  border: 1px solid var(--color-accent);
  border-radius: 12px;
}

.card__status {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  transform: translateY(-50%);
  display: flex;
  place-content: center;
  margin: 0 auto;
  place-items: center;
  width: max-content;
}

.card__status-icon {
  width: 44px;
  height: 44px;
  background-color: var(--color-primary);
}

.card__container::before {
  position: absolute;
  top: 0;
  left: 16px;
  padding-left: 1px;
  transform: translateY(-50%);
  font-family: var(--font-family);
  font-weight: 400;
  font-size: 14px;
  color: var(--color-primary-inverted);
  background-color: var(--color-primary);
  content: counter(list-item, decimal-leading-zero);
}

.card__front,
.card__back {
  height: 100%;
  padding: 28px 19px;
  border-radius: 16px;

  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  backface-visibility: hidden;
}

.card__back {
  background-color: var(--color-primary);
  transform: rotateY(180deg);
}

.card__word,
.card__translation {
  margin: 0;
}

.card__buttons {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 32px;
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  transform: translateY(50%);
  width: max-content;
  margin: 0 auto;
  background-color: var(--color-primary);
  z-index: 1;
}

.card__button {
  display: grid;
  place-items: center;
  padding: 10px 4px;
  font-weight: 700;
  font-size: 12px;
  line-height: 1.5;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--color-font-basic);
  background-color: var(--color-primary);
  border: none;
  cursor: pointer;
}

.card__icon {
  width: 24px;
  height: 24px;
}

.card__status-completed {
  padding: 0 4px;
  font-weight: 700;
  font-size: 12px;
  line-height: 150%;
  letter-spacing: 0.12em;
  color: var(--color-font-basic);
}
</style>
