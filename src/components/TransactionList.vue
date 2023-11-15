<script setup>

  import { defineProps } from 'vue';

  const emits = defineEmits(['transactionDeleted'])

  const props = defineProps({
    transactions : {
        type: Array,
        required: true,
    },
  })

  // delete transaction

  const deleteTransaction = (id) => {
    emits('transactionDeleted', id)

  }

        



</script>

<template>
   <h3>History</h3>
   <ul id="list" class="list">
    <li v-for="transaction in transactions" :key="transaction.id" :class="transaction.amount < 0 ? 'minus' : 'plus'">
    {{ transaction.text }} <span>{{ transaction.amount }}</span><button class="delete-btn" @click="deleteTransaction(transaction.id)">x</button>

    </li>
   
   </ul>
</template>



<style lang="scss" scoped>

</style>