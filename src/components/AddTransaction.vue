<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" placeholder="Enter text..." v-model="text" />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        type="text"
        id="amount"
        placeholder="Enter amount..."
        v-model="amount"
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref } from "vue";
import { useToast } from "vue-toastification";

const text = ref("");
const amount = ref(0);

const emit = defineEmits(["addTransaction"]);

const toast = useToast();

const onSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error("Missing text or amount!");
    return;
  }

  const transaction = {
    id: Math.floor(Math.random() * 100000000),
    text: text.value,
    amount: +amount.value,
  };

  emit("addTransaction", transaction);

  text.value = "";
  amount.value = 0;
};
</script>
