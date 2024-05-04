<template>
    <Header/>
        
      <!-- Other components or content of your app -->
      <div class="container">
        <Balance :total = "total" :income = "income"/>
        <IncomeExp :income = "+income"  :expense = "+expense"/>
        <TransactionList :transaction = "transaction"
        @transactionDelted="handleTransactionDelete"/>

        <AddTransc @transactionSubmit = "handleTransactionSubmit"/>
      </div>
      </template>
  
  <script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExp from './components/IncomeExp.vue';
  import AddTransc from './components/AddTransc.vue';
  import TransactionList from './components/TransactionList.vue';
  import { ref , computed ,onMounted } from 'vue';
  import {useToast} from 'vue-toastification'
  const toast = useToast();


  const transaction = ref([]);

        onMounted(()=>{
            const savedTransaction = JSON.parse(localStorage.getItem
        ('transaction'));
        if(savedTransaction){
            transaction.value = savedTransaction;
        }
        })

        const total = computed(()=>{
            return transaction.value.reduce((acc,transaction)=>{
            return acc + transaction.amount;
            } , 0).toFixed(2);
        })

        const income = computed(()=>{
            return transaction.value
            .filter((transaction)=>transaction.amount > 0)
            .reduce((acc,transaction)=>{
            return acc + transaction.amount;
            } , 0).toFixed(2);
        })

        const expense = computed(()=>{
            return transaction.value
            .filter((transaction)=>transaction.amount < 0)
            .reduce((acc,transaction)=>{
            return acc + transaction.amount;
            } , 0).toFixed(2);
        })

        const handleTransactionSubmit = (transactionData)=>{
            transaction.value.push({
                id:generateUniqueId(),
                text: transactionData.text,
                amount : transactionData.amount

            });
            saveTransactionLocal();
            toast.success('Transaction Added')
        }
        const generateUniqueId = ()=>{
            return Math.floor(Math.random()*100000)
        }
        const handleTransactionDelete = (id) => {
    transaction.value = transaction.value.filter((transaction) => transaction.id !== id); // Added return statement here
    saveTransactionLocal();
    toast.success("Transaction Deleted");
  }; // Added closing bracket here

  const saveTransactionLocal = () => {
    localStorage.setItem('transaction', JSON.stringify(transaction.value)); // Fixed the typo and added the correct key 'transaction'
  }
  
  </script>