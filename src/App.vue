<template>
  <Header />
  <div class="container">
  <Balance :total="+total"/>
  <IncomeExpense :income="+income" :expense="+expense" />
  <History :transactions="transactions" @removeTransaction="handleRemoveTransaction"/>
  <AddNew @transactionSubmitted="handleTransactionSubmitted"/>
  </div>
</template>

<script setup>
import Header from './components/Header.vue'
import Balance from './components/Balance.vue'
import IncomeExpense from './components/IncomeExpense.vue'
import History from './components/History.vue'
import AddNew from './components/AddNew.vue'

import { ref, computed, onMounted } from 'vue'
import { useToast } from 'vue-toastification'

const toast = useToast();

const transactions = ref ([]);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'))

  if(savedTransactions)
  {
    transactions.value = savedTransactions;
  }
})

 const total = computed(() => {
  return transactions.value.reduce((acc, transactions) => {
    return acc + parseFloat(transactions.amount)
  }, 0)
 })

 const income = computed(() => {
  return transactions.value
  .filter(transaction => transaction.amount > 0)
  .reduce((acc, transaction) => {
    return acc + parseFloat(transaction.amount)
  }, 0).toFixed(2)
 })

 const expense = computed(() => {
  return transactions.value
  .filter(transactions => transactions.amount < 0)
  .reduce((acc, transactions) => {
    return acc + parseFloat(transactions.amount)
  }, 0).toFixed(2)
 })

 const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateRandomId(),
    text: transactionData.text,
    amount: transactionData.amount
  })
  saveTransactionsToLocalStorage();
  toast.success('You add a Transaction');
 }

 const generateRandomId = () => {
  return Math.floor(Math.random() * 1000000) + 1;
 }

const handleRemoveTransaction = (id) => {
  transactions.value = transactions.value.filter(transaction => transaction.id !== id)
  saveTransactionsToLocalStorage();
}

const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

</script>