<script setup>
import { ref, reactive, computed } from "vue";

const header = ref("Shopping List App");
const editing = ref(false);
const items = ref([
  { id: 1, label: "Test item 1", purchased: true, highPriority: false },
  { id: 2, label: "Test item 1", purchased: true, highPriority: true },
  { id: 3, label: "Test item 1", purchased: false, highPriority: true },
]);
const reversedItems = computed(() => [...items.value].reverse());
const newItem = ref("");
const newItemHighPriority = ref(false);
const toggleEdition = () => {
  editing.value = !editing.value;
  newItem.value = "";
  newItemHighPriority.value = false;
};
const saveItems = () => {
  items.value.push({
    id: items.value.length + 1,
    label: newItem.value,
    highPriority: newItemHighPriority.value,
  });
  newItem.value = "";
  newItemHighPriority.value = false;
};
const togglePurchased = (item) => {
  item.purchased = !item.purchased;
};

// reactive

// const state = reactive({ count: 0 });
// const increment = () => {
//   state.count++;
// };
</script>

<template>
  <div class="header">
    <h1>{{ header.toLocaleUpperCase() }}</h1>
    <button class="btn" v-if="editing" @click="toggleEdition">Cancel</button>
    <button class="btn btn-primary" v-else @click="toggleEdition">
      Add Item
    </button>
  </div>
  <form class="add-item-form" v-if="editing" @submit.prevent="saveItems">
    <input v-model.trim="newItem" type="text" placeholder="Add an item" />
    <label>
      <input v-model="newItemHighPriority" type="checkbox" />
      High priority
    </label>
    <button class="btn btn-primary" :disabled="newItem.length === 0">
      Save Item
    </button>
  </form>
  <p v-if="!items.length">Nothing to see here</p>
  <ul>
    <li
      v-for="(item, index) in reversedItems"
      :key="item.id"
      :class="item.purchased ? 'strikeout' : item.highPriority && 'priority'"
      @click="togglePurchased(item)"
    >
      {{ item.label }}
    </li>
  </ul>
  <!-- <div>
    <h1>
      {{ state.count }}
    </h1>
    <button @click="increment">Increment count</button>
  </div> -->
</template>
