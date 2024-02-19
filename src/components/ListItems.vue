<script setup>
import { ref } from 'vue'
import Delete from './Delete.vue'

const props = defineProps({ list: Object })
const editing = ref(false)

const togglePurchased = (items) => {
  items.purchased = !items.purchased
  purchasedItem(props.list)
}

const doEdit = (e) => {
  editing.value = e
}
const purchasedItem = (list) => {
  fetch('http://localhost:3000/shoppinglists/' + props.list._id, {
    method: 'PATCH',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      title: props.list.title,
      items: [...props.list.items],
      updatedAt: new Date()
    })
  })
    .then((res) => res.json())
    .then((r) => {})
}
</script>

<template>
  <div>
    <h3>{{ props.list.title }}: {{ props.list.items.length }} Items</h3>
    <!-- {{ props.list.updatedAt }} -->
    <div>
      <button v-if="editing" @click="doEdit(false)">Hide List</button>
      <button v-else @click="doEdit(true)">Show List</button>
      <div v-if="editing">
        <ul>
          <li
            v-for="items in props.list.items"
            @click="togglePurchased(items)"
            :key="list.items.id"
            :class="{ strikeout: items.purchased }"
          >
            {{ items.name }}
            <input v-model="items.purchased" type="checkbox" />
          </li>
        </ul>
        <!-- <delete :list="props.list" /> -->
      </div>
    </div>
  </div>
</template>

<style>
.strikeout {
  text-decoration: line-through;
  color: #b8c2cc;
}

.strikeout:hover {
  color: #8795a1;
}
</style>
