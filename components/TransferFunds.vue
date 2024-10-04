<template>
    <div class="flex items-center justify-center bg-gray-100">
      <div class="w-full max-w-4xl p-8 bg-white shadow-lg rounded-lg">
        <h2 class="text-3xl font-bold mb-8 text-center text-gray-800">Переказ з картки на картку</h2>
        <form @submit.prevent="transferFunds" class="flex flex-col md:flex-row justify-between items-center space-y-4 md:space-y-0 md:space-x-4">
          <div class="w-full md:w-1/3 bg-gradient-to-r from-blue-500 to-blue-700 p-4 rounded-lg shadow-md">
            <h3 class="text-xl font-semibold mb-2 text-white">З картки</h3>
            <select v-model="selectedFromCard" class="w-full p-2 border border-gray-300 rounded mb-2 focus:outline-none focus:ring-2 focus:ring-blue-500">
              <option v-for="card in cards" :key="card.number" :value="card">
                {{ card.name }} *{{ card.number.slice(-4) }} - Bal: {{ card.balance }}₴
              </option>
            </select>
          </div>
          <div class="w-full md:w-1/3 text-center">
            <label for="amount" class="block text-xl font-semibold mb-2 text-gray-700">Сума</label>
            <input type="number" id="amount" v-model="amount" required class="w-full p-2 border border-gray-300 rounded mb-2 focus:outline-none focus:ring-2 focus:ring-blue-500" />
            <button type="submit" class="w-full bg-green-500 text-white p-2 rounded hover:bg-green-600 transition duration-300" :disabled="loading">
              <span v-if="loading" class="flex items-center justify-center">
                <svg class="animate-spin h-5 w-5 mr-3 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                  <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                  <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                </svg>
                Переказю...
              </span>
              <span v-else>Переказати</span>
            </button>
          </div>
          <div class="w-full md:w-1/3 bg-gradient-to-r from-green-500 to-green-700 p-4 rounded-lg shadow-md">
            <h3 class="text-xl font-semibold mb-2 text-white">Картка одержувача</h3>
            <select v-model="selectedToCard" class="w-full p-2 border border-gray-300 rounded mb-2 focus:outline-none focus:ring-2 focus:ring-blue-500">
              <option v-for="card in cards" :key="card.number" :value="card">
                {{ card.name }} *{{ card.number.slice(-4) }} - Bal: {{ card.balance }}₴
              </option>
            </select>
          </div>
        </form>
        <div v-if="message" class="mt-4 text-center text-green-500 font-semibold">{{ message }}</div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, watch } from 'vue';
  
  const props = defineProps({
    cards: {
      type: Array,
      required: true,
      default: () => []
    }
  });
  
  const emit = defineEmits(['update-cards', 'add-transaction']);
  
  const selectedFromCard = ref(props.cards[0] || {});
  const selectedToCard = ref(props.cards[1] || {});
  const amount = ref(0);
  const message = ref('');
  const loading = ref(false);
  
  const saveCardsToLocalStorage = (cards) => {
    localStorage.setItem('cards', JSON.stringify(cards));
  };
  
  const transferFunds = () => {
    loading.value = true;
    setTimeout(() => {
      if (amount.value > selectedFromCard.value.balance) {
        message.value = 'Недостаточно средств на карте отправителя.';
      } else {
        selectedFromCard.value.balance -= amount.value;
        selectedToCard.value.balance += amount.value;
        message.value = `Перевод ${amount.value}₴ с карты ${selectedFromCard.value.name} на карту ${selectedToCard.value.name} успешно выполнен!`;
        emit('update-cards', props.cards);
        saveCardsToLocalStorage(props.cards);
  
        // Добавление транзакции
        const transaction = {
          from: selectedFromCard.value.name,
          to: selectedToCard.value.name,
          amount: amount.value,
          date: new Date().toLocaleString()
        };
        emit('add-transaction', transaction);
      }
      loading.value = false;
    }, 2000); // Имитация задержки в 2 секунды
  };
  
  watch([selectedFromCard, selectedToCard], () => {
    emit('update-cards', props.cards);
  });
  </script>
  
  <style scoped>
  /* TailwindCSS classes are used directly в the template */
  </style>
  