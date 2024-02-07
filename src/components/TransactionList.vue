<template>
  <h3>History</h3>

  <ul class="list">
    <li v-for="{ id, text, amount } in transactions" :key="id" :class="amount < 0 ? 'minus' : 'plus'" class="list">
      {{ text }}
      <span>{{ amount < 0 ? "-" : "" }}${{ Math.abs(amount) }}</span>
      <button @click="emit('transactionDeleted', id)" class="delete-btn">x</button>
    </li>
  </ul>
</template>

<script setup>
defineProps({
  transactions: { type: Array, required: true },
})
const emit = defineEmits(["transactionDeleted"])
</script>