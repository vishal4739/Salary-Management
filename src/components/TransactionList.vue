<template>
    <h3>History</h3>
      <ul id="list" class="list">
        <li v-for="trans in transaction" :class="trans.amount < 0 ? 'minus' : 'plus'">
          {{ trans.text }} <span>{{ trans.amount }}</span>
          <button class="delete-btn" @click="deleteTransaction(trans.id)">x</button>

        </li>
      
      </ul>
</template>

<script setup>
import { defineProps } from 'vue';

const emit = defineEmits(['transactionDelted'])
const props = defineProps({
  transaction :{
  type : Array,
  required : true
  },
})
const deleteTransaction = (id)=>{
emit('transactionDelted',id)
}
</script>