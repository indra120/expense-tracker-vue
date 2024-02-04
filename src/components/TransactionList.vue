<template>
  <h3>History</h3>

  <ul id="list" class="list">
    <li
      class="list"
      v-for="{ id, text, amount } in transactions"
      :key="id"
      :class="amount < 0 ? 'minus' : 'plus'"
    >
      {{ text }}
      <span>
        {{ amount < 0 ? "-" : "" }}$
        {{ Math.abs(amount) }}
      </span>
      <button @click="deleteTransaction(id)" class="delete-btn">x</button>
    </li>
  </ul>
</template>

<script setup>
const emit = defineEmits(["transactionDeleted"])

const props = defineProps({
  transactions: { type: Array, required: true },
})

const deleteTransaction = (id) => {
  emit("transactionDeleted", id)
}
</script>
