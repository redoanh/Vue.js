<script setup>
import { ref, computed } from "vue";

const props = defineProps(["transactions"]);
const emit = defineEmits(["delete-transaction"]);

const filter = ref("All");

const filteredTransactions = computed(() => {
  if (filter.value === "Income") {
    return props.transactions.filter((t) => t.type === "Income");
  }
  if (filter.value === "Expense") {
    return props.transactions.filter((t) => t.type === "Expense");
  }
  return props.transactions;
});

const deleteTransaction = (index) => {
  emit("delete-transaction", index);
};
</script>

<template>
    <div class="transaction-list card shadow-lg p-4 mt-4">
      <h3 class="text-center text-primary fw-bold">
        <i class="bi bi-cash-stack"></i> Transactions
      </h3>
      <hr />

      <div class="mb-3 d-flex justify-content-end align-items-center">
        <label class="fw-bold me-2"><i class="bi bi-filter"></i> Filter: </label>
        <select v-model="filter" class="form-select custom-select w-auto">
          <option value="All">All</option>
          <option value="Income" class="text-success">Income</option>
          <option value="Expense" class="text-danger">Expense</option>
        </select>
      </div>
  
      <table class="table table-hover table-bordered text-center mt-3" v-if="filteredTransactions.length">
        <thead class="table-dark">
          <tr>
            <th>Title</th>
            <th>Amount</th>
            <th>Type</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(transaction, index) in filteredTransactions" :key="index">
            <td class="fw-bold">{{ transaction.title }}</td>
            <td
              :class="{
                'text-success fw-bold': transaction.type === 'Income',
                'text-danger fw-bold': transaction.type === 'Expense' && transaction.amount >= 500,
                'text-danger': transaction.type === 'Expense',
              }"
            >
              {{ transaction.amount }}
            </td>
            <td>
              <span :class="transaction.type === 'Income' ? 'badge bg-success' : 'badge bg-danger'">
                {{ transaction.type }}
              </span>
            </td>
            <td>
              <button class="btn btn-sm btn-danger shadow-sm" @click="deleteTransaction(index)">
                <i class="bi bi-trash3"></i> Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
  
      
      <p v-else class="text-muted text-center fs-5">
        <i class="bi bi-info-circle"></i> No transactions recorded yet.
      </p>
    </div>
  </template>
  



