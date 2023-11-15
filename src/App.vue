<script setup>
 import HeaderNav from './components/HeaderNav.vue';
 import Balance from './components/Balance.vue';
 import IncomeExpenses from './components/IncomeExpenses.vue';
 import TransactionList from './components/TransactionList.vue';
 import AddTransaction from './components/AddTransaction.vue';

import { ref, computed, onMounted} from 'vue';

import { useToast } from 'vue-toastification';


const toast = useToast()


//  transaction list

const transactions = ref([])

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'))

  if (savedTransactions){
    transactions.value = savedTransactions
  }
})


// Get total

const total = computed(() => {
  return transactions.value.reduce((acc,transaction) => {
   return acc + transaction.amount;
  },0)
} )

// Get Income 

const income = computed(() => {
  return transactions.value.filter( (transaction) => transaction.amount > 0 ) . reduce((acc,transaction) => {
    return acc + transaction.amount;
  },0 ) .toFixed(2)
})




// Get Expenses

const expenses  = computed (() => {
  return transactions.value.filter( (transaction) =>
    transaction.amount < 0
   ) .reduce((acc, transaction) => {
    return acc + transaction.amount
  },0 ) .toFixed(2)
} );



// Add transaction

const handleTransactionSubmitted =(transactionData) => {
   transactions.value.push(
    {
    id: generateUniqueId(),
    text:transactionData.text,
    amount:transactionData.amount,

   });

  //  save to localStorage

  saveToLocalStorage();

   toast.success('Transaction added');

};

// Generate Unique Id
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
};



// Delete transaction

const handleTransactionDeleted = (id) =>{
  transactions.value = transactions.value.filter ((transaction) => transaction.id !== id );
   
  // save to localStorage

  saveToLocalStorage();

  toast.success(" Transaction deleted" );

}

// save transactions to localStorage


const saveToLocalStorage = () =>{
  localStorage.setItem('transactions' , JSON.stringify(transactions.value))
}



</script>


<template>
  <HeaderNav />
  <div class="container">
    <Balance :total = "+total" />
    <IncomeExpenses :income = "+income" :expenses = "+expenses" />
    <TransactionList :transactions = "transactions" @transactionDeleted="handleTransactionDeleted"/>
    <AddTransaction @transactionSubmitted = "handleTransactionSubmitted"/>
  </div>
</template>



<style lang="scss" scoped>

</style>