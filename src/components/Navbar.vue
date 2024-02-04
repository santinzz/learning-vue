<script setup>
import { defineEmits } from 'vue'
import NavbarSvg from './NavbarSvg.vue'
import DeleteBoardButton from './DeleteBoardButton.vue'

const emits = defineEmits(['create-board, delete-board'])

const handleClick = () => {
  emits('create-board')
}

const onDeleteBoard = () => {
  emitsDelete('delete-board', 'boardId')
}
</script>

<template>
  <nav>
    <a class="logo-anchor" href="#">
      <NavbarSvg />
    </a>
    <div class="board-buttons">
      <a class="create-board-anchor" href="#" @click="handleClick">Create Board</a>
      <DeleteBoardButton @delete-board-button-clicked="onDeleteBoard"/>
    </div>
</nav>
</template>

<style scoped>
nav {
  display: flex;
  justify-content: flex-end;
  padding: 20px;
  background-color: #007bff;
  justify-content: space-between;
}

.create-board-anchor {
  color: #fff;
  text-decoration: none;
  padding: 8px 12px;
  border-radius: 4px;
  transition: .2s;
  border: #fff 1px solid;
  display: flex;
  align-items: center;
}

.create-board-anchor:hover {
  border: 1px solid #6be08e;
  background-color: #6be08e;
}

.logo-anchor {
  display: flex;
  align-items: center;
  color: #fff;
  text-decoration: none;
}

.board-buttons {
  display: flex;
  align-items: center;
  gap: 1rem;
}
</style>