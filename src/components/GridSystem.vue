<script setup lang="ts">
import { ref } from 'vue';
import { GridLayout, GridItem } from 'vue3-grid-layout-next';

interface GridCard {
  i: string;
  x: number;
  y: number;
  w: number;
  h: number;
  content: string;
}

const layout = ref<GridCard[]>([
  { i: '1', x: 0, y: 0, w: 2, h: 2, content: 'Card 1' },
  { i: '2', x: 2, y: 0, w: 2, h: 2, content: 'Card 2' },
  { i: '3', x: 4, y: 0, w: 2, h: 2, content: 'Card 3' },
]);

const gridColumns = 12;
const rowHeight = 30;
const containerHeight = ref(400);

const updateLayout = (newLayout: GridCard[]) => {
  const maxY = Math.max(...newLayout.map(item => item.y + item.h));
  const newHeight = (maxY + 2) * rowHeight;
  if (newHeight > containerHeight.value) {
    containerHeight.value = newHeight;
  }
  layout.value = newLayout;
};

const addNewCard = () => {
  const newCardId = (layout.value.length + 1).toString();
  const lastCard = layout.value[layout.value.length - 1];
  
  // Calculate position for the new card
  let newX = 0;
  let newY = 0;
  
  if (lastCard) {
    if (lastCard.x + lastCard.w + 2 <= gridColumns) {
      // Place next to the last card if there's space
      newX = lastCard.x + lastCard.w;
      newY = lastCard.y;
    } else {
      // Place on the next row
      newX = 0;
      newY = lastCard.y + lastCard.h;
    }
  }

  const newCard: GridCard = {
    i: newCardId,
    x: newX,
    y: newY,
    w: 2,
    h: 2,
    content: `Card ${newCardId}`
  };

  layout.value.push(newCard);
};
</script>

<template>
  <div>
    <button @click="addNewCard" class="add-card-button">Add New Card</button>
    <div class="grid-container" :style="{ height: containerHeight + 'px' }">
      <GridLayout
        v-model:layout="layout"
        :col-num="gridColumns"
        :row-height="rowHeight"
        :is-draggable="true"
        :is-resizable="true"
        :vertical-compact="true"
        :use-css-transforms="true"
        @layout-updated="updateLayout"
      >
        <GridItem
          v-for="item in layout"
          :key="item.i"
          :x="item.x"
          :y="item.y"
          :w="item.w"
          :h="item.h"
          :i="item.i"
          :min-w="1"
          :min-h="1"
        >
          <div class="grid-card">
            <div class="card-header">{{ item.content }}</div>
            <div class="card-content">
              Drag to move
              <br>
              Resize from corners
            </div>
          </div>
        </GridItem>
      </GridLayout>
    </div>
  </div>
</template>

<style scoped>
.add-card-button {
  margin-bottom: 20px;
  padding: 10px 20px;
  background-color: #4a5568;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
  transition: background-color 0.2s;
}

.add-card-button:hover {
  background-color: #2d3748;
}

.grid-container {
  width: 100%;
  background: #f5f5f5;
  padding: 20px;
  overflow: hidden;
  border-radius: 8px;
}

.grid-card {
  width: 100%;
  height: 100%;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.card-header {
  background: #4a5568;
  color: white;
  padding: 8px;
  font-weight: bold;
}

.card-content {
  padding: 12px;
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  color: #4a5568;
}
</style>