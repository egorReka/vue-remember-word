<script setup>
import Button from './components/Button.vue';
</script>

<template>
  <header></header>
  <main class="main">
    <Button>Начать игру</Button>
  </main>
</template>

<style scoped>
.main {
  display: grid;
  place-content: center;
  min-height: 100vh;
}
</style>
