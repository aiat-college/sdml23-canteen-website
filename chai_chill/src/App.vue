<script setup>
import { ref, onMounted } from 'vue';

const menu = ref([]);
const categories = ref([]);

const fetchMenu = async () => {
  try {
    const response = await fetch('https://script.google.com/macros/s/AKfycbwZ-y4WExGhXtRXiZbEHyuYWFi5xeKSv3BN7xHzpXkNRjpURirNKPhWRN6t0Aib6GnuPQ/exec'); // Replace with your actual Web App URL
    const data = await response.json();
    menu.value = data;
    
    // Extract unique categories
    categories.value = [...new Set(data.map(item => item.category))];
  } catch (error) {
    console.error('Error fetching menu:', error);
  }
};

onMounted(fetchMenu);
</script>

<template>
  <div class="canteen-container">
    <h1>🍽️ Canteen Menu</h1>
    <p v-if="menu.length === 0">Loading menu...</p>
    <div v-else>
      <div v-for="category in categories" :key="category" class="category-section">
        <h2>{{ category }}</h2>
        <div class="menu-grid">
          <div v-for="(item, index) in menu.filter(i => i.category === category)" :key="index" class="menu-item">
            <p><strong>{{ item.name }}</strong></p>
            <p class="price">₹{{ item.price }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.canteen-container {
  text-align: center;
  padding: 20px;
  background: #fff5e1;
  color: #d35400;
}

.category-section {
  margin-bottom: 20px;
  background: #ffefdb;
  padding: 10px;
  border-radius: 8px;
}

.menu-grid {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.menu-item {
  border: 2px solid #ff9500;
  background: #ffe4b5;
  padding: 10px;
  margin: 10px;
  border-radius: 8px;
  transition: transform 0.2s;
}

.menu-item:hover {
  transform: scale(1.05);
  box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.2);
}

.price {
  color: #c0392b;
  font-weight: bold;
}
</style>
