<script setup lang="ts">
import GridSystem from './components/GridSystem.vue';
</script>

<template>
  <div class="app-container">
    <h1>Grid Cards System</h1>
    <GridSystem />
  </div>
</template>

<style>
.app-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

h1 {
  color: #2d3748;
  margin-bottom: 20px;
}
</style>