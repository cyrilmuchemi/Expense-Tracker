<script setup>
import HeaderTemp from './components/HeaderTemp.vue'
import BalanceTemp from './components/BalanceTemp.vue'
import incomeExpense from './components/IncomeExpense.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'
import { ref, computed, onMounted } from 'vue'
import { useToast } from 'vue-toastification'

const toast = useToast()
const transactions = ref([])

onMounted(() => {
  const savedTansactions = JSON.parse(localStorage.getItem('transactions'))
  if (savedTansactions) {
    transactions.value = savedTansactions
  }
})

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
  return Math.floor(Math.random() * 10000)
}

const handleTransactionSubitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount
  })
  saveTransactionsToLocal()

  toast.success('Transaction Added')
}

const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id)

  saveTransactionsToLocal()

  toast.success('Transaction deleted.')
}

//save to local storage
const saveTransactionsToLocal = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
</script>

<template>
  <HeaderTemp />
  <div class="container">
    <BalanceTemp :total="total" />
    <incomeExpense :income="+income" :expenses="+expenses" />
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
    <AddTransaction @transactionSubmitted="handleTransactionSubitted" />
  </div>
</template>
