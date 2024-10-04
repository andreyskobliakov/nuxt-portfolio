<template>
    <div class="flex items-center justify-center bg-gray-100">
      <div class="w-full mt-4 max-w-4xl p-8 bg-white shadow-lg rounded-lg">
        <h2 class="text-3xl font-bold mb-8 text-center text-gray-800">Список транзакцій</h2>
        <div v-if="transactions.length === 0" class="text-center text-gray-700">
          У Вас транзакцій немає
        </div>
        <div v-else class="overflow-x-auto">
          <table class="min-w-full bg-white">
            <thead>
              <tr>
                <th class="py-2 px-4 border-b-2 border-gray-200 bg-gray-100 text-left text-sm leading-4 font-medium text-gray-600 uppercase tracking-wider">З картки</th>
                <th class="py-2 px-4 border-b-2 border-gray-200 bg-gray-100 text-left text-sm leading-4 font-medium text-gray-600 uppercase tracking-wider">На картку</th>
                <th class="py-2 px-4 border-b-2 border-gray-200 bg-gray-100 text-left text-sm leading-4 font-medium text-gray-600 uppercase tracking-wider">Сума</th>
                <th class="py-2 px-4 border-b-2 border-gray-200 bg-gray-100 text-left text-sm leading-4 font-medium text-gray-600 uppercase tracking-wider">Дата</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="transaction in transactions" :key="transaction.date">
                <td class="py-2 px-4 border-b border-gray-200">{{ transaction.from }}</td>
                <td class="py-2 px-4 border-b border-gray-200">{{ transaction.to }}</td>
                <td class="py-2 px-4 border-b border-gray-200">{{ transaction.amount }}₴</td>
                <td class="py-2 px-4 border-b border-gray-200">{{ transaction.date }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  
  const props = defineProps({
    transactions: {
      type: Array,
      required: true,
      default: () => []
    }
  });
  </script>
  
  <style scoped>
  /* TailwindCSS classes are used directly в the template */
  </style>
  