<script setup>
import HeaderTemp from './components/HeaderTemp.vue'
import BalanceTemp from './components/BalanceTemp.vue'
import incomeExpense from './components/IncomeExpense.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'
import { ref, computed } from 'vue'
import { useToast } from 'vue-toastification'

const toast = useToast()
const transactions = ref([
  { id: 1, text: 'Flower', amount: -19.99 },
  { id: 2, text: 'Salary', amount: 299.97 },
  { id: 3, text: 'Book', amount: -10 },
  { id: 4, text: 'Web Dev Gig', amount: 200.1 }
])

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount
  }, 0)
})

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2)
})

const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2)
})

const generateUniqueId = () => {
  return Math.floor(Math.random * 10000)
}

const handleTransactionSubitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount
  })
  if (typeof transactionData.amount === 'number') {
    toast.success('Transaction Added')
  }
}
</script>

<template>
  <HeaderTemp />
  <div class="container">
    <BalanceTemp :total="total" />
    <incomeExpense :income="+income" :expenses="+expenses" />
    <TransactionList :transactions="transactions" />
    <AddTransaction @transactionSubmitted="handleTransactionSubitted" />
  </div>
</template>
