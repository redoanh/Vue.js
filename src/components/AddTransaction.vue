<script setup>
import { ref } from "vue";

const emit = defineEmits(["add-transaction"]);

const title = ref("");
const amount = ref("");
const type = ref("Income");
const errorMessage = ref("");

const addTransaction = () => {
    if (!title.value || amount.value <= 0) {
        errorMessage.value = "Title is required & Amount must be greater than 0!";
        return;
    }

    const newTransaction = {
        title: title.value,
        amount: parseFloat(amount.value),
        type: type.value,
    };

    emit("add-transaction", newTransaction);

    title.value = "";
    amount.value = "";
    type.value = "Income";
    errorMessage.value = "";
};
</script>

<template>
    <div class="card shadow-lg p-4 mt-4 gradient-bg">
        <h3 class="text-center text-light fw-bold">
            <i class="bi bi-wallet2"></i> Add New Transaction
        </h3>
        <hr class="text-light" />

        <div v-if="errorMessage" class="alert alert-danger d-flex align-items-center">
            <i class="bi bi-exclamation-circle me-2"></i> {{ errorMessage }}
        </div>

        <form @submit.prevent="addTransaction">

            <div class="mb-3">
                <label class="form-label text-light fw-bold"><i class="bi bi-pencil"></i> Title:</label>
                <input type="text" v-model="title" class="form-control shadow-sm custom-input"
                    placeholder="Enter transaction title..." />
            </div>

            <div class="mb-3">
                <label class="form-label text-light fw-bold"><i class="bi bi-cash-coin"></i> Amount:</label>
                <div class="input-group">

                    <input type="number" v-model="amount" class="form-control shadow-sm custom-input"
                        placeholder="Enter amount..." />
                </div>
            </div>

            <div class="mb-3">
                <label class="form-label text-light fw-bold">
                    <i class="bi bi-list-check"></i> Type:
                </label>
                <select v-model="type" class="form-select shadow-sm custom-input"
                    :class="type === 'Income' ? 'text-success' : 'text-danger'">
                    <option value="Income" class="text-success">Income</option>
                    <option value="Expense" class="text-danger">Expense</option>
                </select>
            </div>


            <div class="text-center">
                <button class="btn btn-warning fw-bold px-4 py-2 shadow-lg">
                    <i class="bi bi-plus-circle"></i> Add Transaction
                </button>
            </div>
        </form>
    </div>
</template>
