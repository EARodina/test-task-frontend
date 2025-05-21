<script setup>
import { ref, computed } from 'vue';

const ownItems  = ref([
  { id: 1, name: "Shoes 1" },
  { id: 2, name: "Shoes 2" },
  { id: 3, name: "Shoes 3" },
  { id: 4, name: "Shoes 4" },
  { id: 5, name: "T-shirt 1" },
  { id: 6, name: "T-shirt 2" },
  { id: 7, name: "T-shirt 3" },
  { id: 8, name: "T-shirt 4" }
]);

const availableItems = ref([
  { id: 11, name: "Jacket 1" },
  { id: 12, name: "Jacket 2" },
  { id: 13, name: "Jacket 3" },
  { id: 14, name: "Jacket 4" },
  { id: 15, name: "Hoodie 1" },
  { id: 16, name: "Hoodie 2" },
  { id: 17, name: "Hoodie 3" },
  { id: 18, name: "Hoodie 4" }
]);

const selectedOwnItems = ref([]);
const selectedAvailableItem = ref(null);

const selectedCount = computed(() => selectedOwnItems.value.length)

const toggleOwnItem = (item) => {
  const idx = selectedOwnItems.value.findIndex(i => i.id === item.id);
  
  if (idx >= 0) {
    selectedOwnItems.value.splice(idx, 1);
  } else if (selectedOwnItems.value.length < 6) {
    selectedOwnItems.value.push(item);
  }
};

const toggleChoiceItem = (item) => {
  selectedAvailableItem.value =
    selectedAvailableItem.value?.id === item.id ? null : item
};
</script>

<template>
  <div class="container">
    <div class="row">
      <div class="box">
        <div class="list-items">
          <div v-for="item in selectedOwnItems" :key="item.id" class="item">
            {{ item.name }}
          </div>
        </div>
        <span class="label">Selected: {{ selectedCount }} / 6</span>
      </div>
      <div class="box">
        <div v-if="selectedAvailableItem" class="item stretch">
          {{ selectedAvailableItem.name }}
        </div>
      </div>
    </div>

    <div class="row row_bottom">
      <div class="box">
        <div class="list-items">
          <div 
            v-for="item in ownItems" 
            :key="item.id"
            class="item"
            :class="{'selected': selectedOwnItems.includes(item)}"
            @click="toggleOwnItem(item)"
          >
            {{ item.name }}
          </div>
        </div>
      </div>
      <div class="box">
        <div class="list-items">
          <div 
            v-for="item in availableItems" 
            :key="item.id"
            class="item"
            :class="{'selected': item.id === selectedAvailableItem?.id}"
            @click="toggleChoiceItem(item)"
          >
            {{ item.name }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
.container {
  display: flex;
  flex-direction: column;
  gap: 32px;
  height: 100%;
  padding: 16px;
  border: 1px solid black;
}

.row {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 16px;
}

.row_bottom {
  flex-grow: 1;
}

.box{
  display: flex;
  flex-direction: column;
  gap: 24px;
  min-height: 150px;
  padding: 8px;
  border: 1px solid black;
}

.list-items {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 8px;
}

.item {
  padding: 8px;
  border: 1px solid black;
  cursor: pointer;
  transition: all 0.3s;
  text-align: center;
  font-size: 12px;
}

.item.stretch {
  flex-grow: 1;
}

.bottom .item:hover {
  background-color: #e5e3e3;
}

.item.selected {
  background-color: #cbcbcb;
}

.label {
  margin-top: auto;
}
</style>