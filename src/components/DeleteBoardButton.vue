<script setup>
import { ref, defineEmits } from 'vue'

const emits = defineEmits(['delete-board'])

const handleDeleteBoard = () => {
  emits('delete-board-button-clicked')
}

</script>

<template>
  <a href="#" class="delete-board-anchor">
    Delete Board
  </a>
</template>

<style>
.delete-board-anchor {
  color: #fff;
  text-decoration: none;
  padding: 8px 12px;
  border-radius: 4px;
  transition: .2s;
  border: #fff 1px solid;
  display: flex;
  align-items: center;
  transition: .3s;
}

.delete-board-anchor:hover {
  border: 1px solid #d35151;
  background-color: #d35151;
}
</style>
