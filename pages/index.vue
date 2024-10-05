<template>
    <div>
    <CardList :cards="cards" />
    <TransferFunds :cards="cards" @update-cards="updateCards" @add-transaction="addTransaction" />
    <TransactionList :transactions="transactions" />
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  import TransferFunds from './components/TransferFunds.vue';
  import TransactionList from './components/TransactionList.vue';
  import CardList from './components/CardList.vue';
  
  const cards = ref([]);
  const transactions = ref([]);
  
  const loadCardsFromLocalStorage = () => {
    const savedCards = localStorage.getItem('cards');
    if (savedCards) {
      cards.value = JSON.parse(savedCards);
    } else {
      cards.value = [
        { name: 'World Elite', number: '1234 5678 9012 3456', date: '12/25', cvv: '123', balance: 10000, class: 'card-world-elite' },
        { name: 'World Green', number: '2345 6789 0123 4567', date: '11/24', cvv: '234', balance: 8000, class: 'card-world-green' },
        { name: 'World Yellow', number: '3456 7890 1234 5678', date: '10/23', cvv: '345', balance: 6000, class: 'card-world-yellow' },
        { name: 'Black Edition', number: '4567 8901 2345 6789', date: '09/22', cvv: '456', balance: 4000, class: 'card-black-edition' }
      ];
    }
  };
  
  const loadTransactionsFromLocalStorage = () => {
    const savedTransactions = localStorage.getItem('transactions');
    if (savedTransactions) {
      transactions.value = JSON.parse(savedTransactions);
    }
  };
  
  const updateCards = (updatedCards) => {
    cards.value = [...updatedCards];
    localStorage.setItem('cards', JSON.stringify(cards.value));
  };
  
  const addTransaction = (transaction) => {
    transactions.value.push(transaction);
    localStorage.setItem('transactions', JSON.stringify(transactions.value));
  };
  
  onMounted(() => {
    loadCardsFromLocalStorage();
    loadTransactionsFromLocalStorage();
  });
  </script>
  