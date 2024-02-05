<template>
  <h2>Expense Tracker</h2>

  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList
      :transactions="transactions"
      @transaction-deleted="handleDeleteTransaction"
    />
    <AddTransaction @submit-transaction="handleSubmitTransaction" />
  </div>
</template>

<script setup>
import { computed, onMounted, ref, onUpdated } from "vue"
import { useToast } from "vue-toastification"

import Balance from "./components/Balance.vue"
import IncomeExpenses from "./components/IncomeExpenses.vue"
import TransactionList from "./components/TransactionList.vue"
import AddTransaction from "./components/AddTransaction.vue"

const toast = useToast()
const transactions = ref([])

onMounted(() => {
  transactions.value = JSON.parse(localStorage.getItem("transactions")) || []
})

onUpdated(() => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value))
})

const total = computed(() => {
  return transactions.value.reduce((acc, data) => acc + data.amount, 0)
})

const income = computed(() => {
  return transactions.value
    .filter((data) => data.amount > 0)
    .reduce((acc, data) => acc + data.amount, 0)
    .toFixed(2)
})

const expenses = computed(() => {
  return transactions.value
    .filter((data) => data.amount < 0)
    .reduce((acc, data) => acc + data.amount, 0)
    .toFixed(2)
})

const handleSubmitTransaction = (transaction) => {
  transactions.value.push(transaction)
  toast.success("New transaction added")
}

const handleDeleteTransaction = (id) => {
  transactions.value = transactions.value.filter((data) => data.id !== id)
  toast.success("Transaction has been deleted")
}
</script>
