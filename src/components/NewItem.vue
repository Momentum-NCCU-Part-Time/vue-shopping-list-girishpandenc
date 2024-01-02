<script setup>
import { ref } from 'vue'
const newItem = ref('')
const addingItem = ref(false)

const props = defineProps({ list: Object })
const emit = defineEmits(['itemAdded'])

const addNewItem = (list) => {
  fetch('http://localhost:3000/lists/' + props.list.id, {
    method: 'PATCH',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      title: props.list.title,
      items: [
        ...props.list.items,
        {
          id: props.list.items.length + 1,
          itemName: newItem.value,
          purchased: false
        }
      ],
      updatedAt: new Date()
    })
  })
    .then((res) => res.json())
    .then((addedItem) => {
      emit('itemAdded', addedItem)
      resetItem()
    })
}

const addItem = (e) => {
  addingItem.value = e
}

const resetItem = () => {
  newItem.value = ''
}
</script>

<template>
  <div>
    <form v-if="addingItem" class="itemForm" @submit.prevent="addNewItem">
      <input v-model="newItem" type="text" placeholder="Item" />
      <button type="submit">Add</button>
    </form>
    <button v-if="addingItem" @click="addItem(false)">Cancel</button>
    <button v-else @click="addItem(true)">Add Item</button>
  </div>
</template>
