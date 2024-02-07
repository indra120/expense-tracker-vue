<template>
  <h3>Add new transaction</h3>

  <form @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input v-model="text" type="text" id="text" placeholder="Enter text..." />
    </div>

    <div class="form-control">
      <label for="amount">Amount <br />(negative - expense, positive - income)</label>
      <input v-model="amount" type="text" id="amount" placeholder="Enter amount..." />
    </div>

    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref } from "vue"
import { useToast } from "vue-toastification"

const text = ref("")
const amount = ref("")
const emit = defineEmits(["submitTransaction"])
const toast = useToast()

const onSubmit = () => {
  if (!text.value || !amount.value) {
    return toast.error("All fields are required!")
  }

  emit("submitTransaction", {
    id: Math.floor(Math.random() * 69420),
    text: text.value,
    amount: parseFloat(amount.value),
  })

  text.value = ""
  amount.value = ""
}
</script>