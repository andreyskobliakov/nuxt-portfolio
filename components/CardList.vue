<template>
  <div class="flex items-center justify-center bg-gray-100">
    <div class="w-full m-4 max-w-4xl p-8 bg-white shadow-lg rounded-lg">
      <h2 class="text-3xl font-bold mb-8 text-center text-gray-800">Мої картки</h2>
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div v-for="card in cards" :key="card.number" :class="card.class">
          <div class="flex justify-between items-center mb-4">
            <h3 class="text-xl font-semibold">{{ card.name }}</h3>
            <img src="/public/nfs.png" alt="Card Logo" class="w-8 h-8">
          </div>
          <p class="text-lg tracking-widest mb-4"> {{ card.number }}</p>
          <div class="flex justify-between items-center">
            <div>
              <p class="text-sm">VALID THRU</p>
              <p class="text-lg">{{ card.date }}</p>
            </div>
            <div>
              <p class="text-sm">CVV</p>
              <p class="text-lg cursor-pointer" @click="toggleCVV(card)">
                {{ card.showCVV ? card.cvv : '***' }}
              </p>
            </div>
          </div>
          <div class="mt-4">
            <p class="text-lg font-semibold">{{ card.balance }} ₴</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const props = defineProps({
  cards: Array
});

const toggleCVV = (card) => {
  card.showCVV = !card.showCVV;
};
</script>

<style scoped>
.card-world-elite {
  @apply bg-gradient-to-r from-blue-500 to-blue-700 text-white p-6 rounded-lg shadow-lg border border-blue-300;
}

.card-world-green {
  @apply bg-gradient-to-r from-green-500 to-green-700 text-white p-6 rounded-lg shadow-lg border border-green-300;
}

.card-world-yellow {
  @apply bg-gradient-to-r from-yellow-500 to-yellow-700 text-white p-6 rounded-lg shadow-lg border border-yellow-300;
}

.card-black-edition {
  @apply bg-gradient-to-r from-gray-800 to-gray-900 text-white p-6 rounded-lg shadow-lg border border-gray-600;
}
</style>
