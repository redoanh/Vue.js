<script setup>
import { ref, onMounted, computed } from "vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

const transactions = ref([]);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transactions")) || [];
  transactions.value = savedTransactions;
});

const saveTransactions = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};

const addTransaction = (newTransaction) => {
  transactions.value.push(newTransaction);
  saveTransactions();
};

const deleteTransaction = (index) => {
  transactions.value.splice(index, 1);
  saveTransactions();
};
</script>

<template>
  <div class="container mt-5">
    <h2 class="text-center">Expense Tracker</h2>
    <AddTransaction @add-transaction="addTransaction" />
    <TransactionList :transactions="transactions" @delete-transaction="deleteTransaction" />
  </div>
</template>
