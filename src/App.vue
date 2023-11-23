<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList
      :transactions="transactions"
      @deleteTransaction="handleDelete"
    />
    <AddTransaction @addTransaction="handleAddTransaction" />
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { useToast } from "vue-toastification";

import { ref, computed, onMounted } from "vue";

const toast = useToast();

const transactions = ref([]);

onMounted(() => {
  const localTransactions = JSON.parse(localStorage.getItem("transactions"));
  if (localTransactions) {
    transactions.value = localTransactions;
  }
});

const total = computed(() => {
  return transactions.value
    .reduce((acc, item) => (acc += item.amount), 0)
    .toFixed(2);
});

const income = computed(() => {
  return transactions.value
    .filter((item) => item.amount > 0)
    .reduce((acc, item) => (acc += item.amount), 0)
    .toFixed(2);
});

const expenses = computed(() => {
  return transactions.value
    .filter((item) => item.amount < 0)
    .reduce((acc, item) => (acc += item.amount), 0)
    .toFixed(2);
});

const handleAddTransaction = (transaction) => {
  transactions.value.push(transaction);
  updateLocalStorage();
  toast.success("Transaction added!");
};

const handleDelete = (id) => {
  transactions.value = transactions.value.filter((item) => item.id !== id);
  updateLocalStorage();
  toast.success("Transaction deleted!");
};

const updateLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>
