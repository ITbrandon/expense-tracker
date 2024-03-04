<template>
  <h3>History</h3>
      <ul id="list" class="list">
        <li v-for="transaction in transactions" :class="plusminus(transaction)" :key="transaction.id">
          {{transaction.text}} <span>{{transaction.amount}}</span><button @click="removeItem(transaction.id)" class="delete-btn">x</button>
        </li>
      </ul>
</template>

<script setup>

  const emit = defineEmits(['removeTransaction'])

  const props = defineProps({
    transactions: {
      type: Array,
      required: true
    }
  })

  const plusminus = (transaction) => {
    if(transaction.amount < 0)
    {
      return 'minus'
    }

    else
    {
      return 'plus'
    }
  }

  const removeItem = (targetId) => {
     emit('removeTransaction', targetId)
  }

</script>