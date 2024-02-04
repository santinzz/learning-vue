<script setup>
import { ref } from 'vue';

const text = ref('')
const emits = defineEmits(['add-item'])

const handleSubmit = () => {
  if (text !== '' && text.value.trim() !== ''){
    emits('add-item', text.value)
    text.value = ''
  }
}

</script>

<template>
  <form class="inputForm" action="" @submit.prevent="handleSubmit">
    <input class="inputForm-input" type="text" v-model="text">
    <button class="inputForm-button">Add item</button>
  </form>
</template>

<style scoped>
.inputForm {
  display: flex;
  margin-top: 6px;
  margin-bottom: 12px;
}

.inputForm-input, .inputForm-button {
  flex: 1;
  padding: 8px 10px;
  font-size: 0.9em;
}

.inputForm-input {
  border: 1px solid #888;
  border-right: none;
  border-radius: 5px 0 0 5px;
}

.inputForm-button {
  background-color: #007bff;
  color: #fff;
  border: 1px solid #007bff;
  border-radius: 0 5px 5px 0;
  cursor: pointer;
}

.inputForm-button:hover {
  background-color: #0056b3;
}

.inputForm-input:focus {
  outline: none;
}
.inputForm-button:active {
  background-color: #004c9a;
}
</style>