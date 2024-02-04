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
import { computed, onMounted, ref } from "vue"
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

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) =>  acc + transaction.amount, 0)
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

const handleSubmitTransaction = (transaction) => {
  transactions.value.push(transaction)
  saveToLocalStorage()
  toast.success("New transaction added")
}

const handleDeleteTransaction = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  )
  saveToLocalStorage()
  toast.success("Transaction has been deleted")
}

const saveToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value))
}
</script>
