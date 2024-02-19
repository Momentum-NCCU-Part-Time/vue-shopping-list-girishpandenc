<script setup>
import { ref } from 'vue'
const newList = ref('')

const addingList = ref(false)
const emit = defineEmits(['listAdded'])

const addNewList = () => {
  fetch('http://localhost:3000/shoppinglists/', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ title: newList.value, items: [], updatedAt: new Date() })
  })
    .then((res) => res.json())
    .then((list) => {
      /* emit("listAdded", list) */
      resetList()
    })
}

const addList = (e) => {
  addingList.value = e
}

const resetList = () => {
  newList.value = ''
}
</script>

<template>
  <button v-if="addingList" @click="addList(false)">Cancel</button>
  <button v-else @click="addList(true)">New List</button>
  <form v-if="addingList" class="newList" @submit.prevent="addNewList">
    <input v-model="newList" type="text" placeholder="New List Title" />
    <button type="submit">Add</button>
  </form>
</template>
