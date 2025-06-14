<script setup>
import { ref } from 'vue';

import Button from './components/Button.vue';
import Score from './components/Score.vue';
import Card from './components/Card.vue';

let score = ref(100);
const cardData = ref({
  word: 'unadmitted',
  translation: 'непризнанный',
  state: 'closed',
  status: 'pending',
});

const handleFlip = () => {
  cardData.value.state = 'open';
  // убирать обработчик клика после переворота карты
  console.log('Card flipped');
};

const handleRemembered = (isRemembered) => {
  cardData.value.status = isRemembered ? 'success' : 'fail';
};
</script>

<template>
  <header class="header">
    <div class="container">
      <div class="header__container">
        <h1 class="header__title">ЗАПОМНИ СЛОВО</h1>
        <Score :score="score" />
      </div>
    </div>
  </header>

  <main class="main">
    <div class="container">
      <div class="main__container">
        <Button class="main__button">Начать игру</Button>

        <ul class="cards">
          <li class="cards__item">
            <Card
              @flip="handleFlip"
              @isRemembered="handleRemembered"
              v-bind="cardData"
            />
          </li>
        </ul>
      </div>
    </div>
  </main>
</template>

<style scoped>
.main {
  display: grid;
  height: 100%;
}

.main__container {
  display: grid;
  height: 100%;
}

.main__button {
  place-self: center;
}

.header {
  padding: 37px 0;
}

.header__container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.header__title {
  margin: 0;
  font-weight: 700;
  font-size: 16px;
  letter-spacing: 0.12em;
  color: var(--color-font-basic);
}

.cards {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 66px 106px;
  margin: 0;
  padding: 0;
  list-style: none;
  counter-reset: list-item;
}

.cards__item {
  counter-increment: list-item;
  perspective: 1000px;
}
</style>
