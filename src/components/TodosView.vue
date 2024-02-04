<script setup>
import { reactive, ref } from 'vue'
import DeleteButton from './DeleteButton.vue'
import Navbar from './Navbar.vue'
import InputTodos from './InputTodos.vue'

const isCreatingBoard = ref(false)
const isDeletingBoard = ref(false)
const boardName = ref('')

let boards = reactive([])

const handleNewItem = (text, boardId) => {
  const currentBoard = boards.find(board => board.id === boardId)
  currentBoard.items.push({
    id: crypto.randomUUID(),
    text,
  })
}

const handleDeleteItem = (boardId, itemId) => {
  const onDeleteBoard = boards.find(board => board.id === boardId) 
  const onDeleteItem = onDeleteBoard.items.find(item => item.id === itemId)

  onDeleteBoard.items = onDeleteBoard.items.filter(item => item.id !== onDeleteItem.id)
}

const handleNewBoard = () => {
  isCreatingBoard.value = true
}

const handleCreateBoard = () => {
  if (boardName.value) {
    boards.push({
      id: crypto.randomUUID(),
      name: boardName.value,
      items: [],
    })
    boardName.value = ''
    isCreatingBoard.value = false
  }
}

const startDrag = (event, board, item) => {
  event.dataTransfer.setData('text/plain', JSON.stringify({
    boardId: board.id,
    itemId: item.id,
  }))
}

const onDrop = (event, newBoard) => {
  const { boardId, itemId } = JSON.parse(event.dataTransfer.getData('text/plain'))

  const item = boards
    .find((board) => board.id === boardId)
    .items.find((item) => item.id === itemId)

  boards.find((board) => board.id === boardId).items = boards
    .find((board) => board.id === boardId)
    .items.filter((item) => item.id !== itemId)

  boards[newBoard].items.push(item)
}

const handleDeleteBoard = (boardId) => {
  boards.filter(board => board.id !== boardId)

  isDeletingBoard.value = false
}

</script>

<template>
  <div>
    <Navbar @create-board="handleNewBoard" @delete-board="() => isDeletingBoard = true"/>
    <form
      @submit.prevent="handleCreateBoard"
      class="board-creator"
      v-show="isCreatingBoard"
    >
      <input type="text" placeholder="Board name" v-model="boardName" />
      <button class="board-creator-submit-button" type="submit">Create</button>
      <DeleteButton
        class="board-creator-svg"
        @delete-item="isCreatingBoard = false"
      />
    </form>
    <section class="boards-container">
      <strong v-if="boards.length === 0">There are no current boards.</strong>
      <article
        class="board"
        @drop="(event) => onDrop(event, boardIndex)"
        @dragover.prevent
        @dragenter.prevent
        v-for="(board, boardIndex) in boards"
        :key="board.id"
      >
        <h2>{{ board.name }}</h2>
        <DeleteButton v-if="isDeletingBoard" @delete-item="handleDeleteBoard(board.id)" />
        <InputTodos @add-item="(text) => handleNewItem(text, board.id)" />
        <ul>
          <li
            class="item"
            draggable="true"
            @dragstart="(event) => startDrag(event, board, item)"
            v-for="item in board.items"
            :key="item.id"
          >
            <h5>{{ item.text }}</h5>
            <DeleteButton
              @delete-item="handleDeleteItem(board.id, item.id)"
            />
          </li>
        </ul>
      </article>
    </section>
  </div>
</template>

<style scoped>
.boards-container {
  display: flex;
  gap: 1rem;
  margin: 8px;
  justify-content: center;
  flex-wrap: wrap;
}

article {
  padding: 1rem;
  border-radius: 0.5rem;
}

ul {
  list-style: none;
}

.board {
  min-width: 300px;
}

.item {
  padding: 0.5rem;
  border: 1px solid #888;
  border-radius: 0.5rem;
  margin: 0.5rem 0;
  display: flex;
  justify-content: space-between;
}

.board-creator {
  position: absolute;
  right: 1rem;
  border: #007bff 1px solid;
  background-color: #fff;
  padding: 1rem;
  border-radius: 8px;
  display: flex;
  gap: 1rem;
  margin: 1rem;
}

.board-creator input {
  padding: 0.5rem;
  border-radius: 4px;
  border: 1px solid #007bff;
}

.board-creator .board-creator-submit-button {
  padding: 0.5rem 1rem;
  border-radius: 4px;
  border: 1px solid #007bff;
  background-color: #007bff;
  color: #fff;
  transition: 0.2s;
  cursor: pointer;
}

.board-creator .board-creator-submit-button:hover {
  background-color: #0056b3;
}

.board-creator .board-creator-svg {
  position: absolute;
  right: 0;
  top: 0;
}
</style>
