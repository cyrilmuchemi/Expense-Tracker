<script setup>
import HeaderTemp from './components/HeaderTemp.vue'
import BalanceTemp from './components/BalanceTemp.vue'
import incomeExpense from './components/IncomeExpense.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'
</script>

<template>
  <HeaderTemp />
  <div class="container">
    <BalanceTemp />
    <incomeExpense />
    <TransactionList />
    <AddTransaction />
  </div>
</template>
