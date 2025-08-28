<script setup>
import { onMounted, ref } from 'vue';

import Button from './components/Button.vue';
import Score from './components/Score.vue';
import Card from './components/Card.vue';
import { API_ENDPOINT } from './constants';

let score = ref(100);
const cardData = ref([]);
const isPlayGame = ref(false)

const updateScore = (value) => {
  if (value === 'success') {
    score.value += 10;
    
    return;
  }

  if ( value === 'fail') {
    score.value -= 4

    if (score.value <= 0) {
      score.value = 0;
    }
  
    return;
  }
}

const updateCardState = (key, newState) => {
  const card = cardData.value.find((card) => card.word === key);

  if (!card) {
    return;
  }

  card.state = newState;
};

const updateCardStatus = (key, newStatus) => {
  const card = cardData.value.find((card) => card.word === key);

  if (!card) {
    return;
  }

  card.status = newStatus;
  updateScore(newStatus)  
};

async function getRandomWords() {
  try {
    const res = await fetch(API_ENDPOINT);

    cardData.value = (await res.json()).map((card) => ({
      ...card,
      state: 'closed',
      status: 'pending',
    }));
  } catch (err) {
    console.error('Не удалось получить слова:', err);
  }
}

onMounted(() => getRandomWords());
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
        
        <Button v-if="!isPlayGame" class="main__button" @click="() => isPlayGame = !isPlayGame">Начать игру</Button>

        <div v-else>
          <ul class="main__list">
            <li v-for="card in cardData" :key="card.word" class="main__item">
              <Card
                v-bind="card"
                @update:status="updateCardStatus"
                @update:state="updateCardState"
              />
            </li>
          </ul>

          <Button class="main__button" @click="() => getRandomWords()">Начать заново</Button>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
.main {
  display: grid;
  height: 100%;
  padding: 45px 0 65px;
}

.main__container {
  display: grid;
  height: 100%;
}

.main__button {
  place-self: center;
  margin: 0 auto;
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

.main__list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 66px 106px;
  margin: 0 0 100px;
  padding: 0;
  list-style: none;
  counter-reset: list-item;
}

.main__item {
  counter-increment: list-item;
  perspective: 1000px;
}
</style>
