<template>
  <HeadExample />
  <div class="container">
    <BalanceDisplay :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList  :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
    <AddTransaction @transactionSubmitted="handleTransactionSubmmitted"/>
  </div>
</template>


<script setup>
import HeadExample from './components/Header.vue';
import BalanceDisplay from './components/BalanceDisplay.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

import {ref, computed, onMounted} from 'vue';
import {useToast} from 'vue-toastification';

const toast = useToast()

const transactions = ref([]);

onMounted(() => {
  const saveTransactions = JSON.parse(localStorage.getItem('transactions'))
  if(saveTransactions)
  {
    transactions.value = saveTransactions
  }
})

// Save to localstorage
const saveTransactionsToLocalStorage = () =>
{
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

// Get total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
})

// Get income
const income = computed(() => {
  return transactions.value
  .filter((transaction) => transaction.amount > 0)
  .reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0)
  .toFixed(2);
})

// Get expenses
const expenses = computed(() => {
  return transactions.value
  .filter((transaction) => transaction.amount < 0)
  .reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0)
  .toFixed(2);
})

const handleTransactionSubmmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount
  })
  toast.success('Transaction added')
  saveTransactionsToLocalStorage()
}

const generateUniqueId = () =>
{
  return Math.floor(Math.random()* 1000000)
}

const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter((transaction) =>
  {
    return transaction.id !== id
  })
  toast.success('Transaction deleted')
  saveTransactionsToLocalStorage()
}


</script>

<style>

</style>
