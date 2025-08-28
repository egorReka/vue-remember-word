<script setup></script>

<template>
  <button class="button">
    <slot></slot>
  </button>
</template>

<style scoped>
.button {
  display: block;
  min-height: 68px;
  min-width: 335px;
  padding: 16px 32px;
  font-family: var(--font-family);
  font-size: 24px;
  text-align: center;
  background-color: var(--color-bg-button);
  border: none;
  border-radius: 100px;
  color: var(--color-primary);
  cursor: pointer;
  transition: opacity 0.2s ease;
}

.button:hover {
  opacity: 0.8;
}
</style>
