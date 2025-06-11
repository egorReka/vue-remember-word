<script setup>
import Heart from '../icons/Heart.vue';

const props = defineProps({
  score: Number,
});
</script>

<template>
  <div class="score">
    {{ props.score }}
    <Heart />
  </div>
</template>

<style scoped>
.score {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 12px 24px;
  font-weight: 700;
  font-size: 16px;
  line-height: 1.5;
  color: var(--color-bg-button);
  background-color: var(--color-accent);
  border-radius: 36px;
  box-shadow: 0 0 4px 0 rgba(0, 0, 0, 0.1);
}
</style>
