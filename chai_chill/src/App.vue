<!-- <script setup>
import { ref, onMounted } from 'vue';

const menu = ref([]);
const categories = ref([]);

const fetchMenu = async () => {
  try {
    const response = await fetch('https://script.google.com/macros/s/AKfycbwZ-y4WExGhXtRXiZbEHyuYWFi5xeKSv3BN7xHzpXkNRjpURirNKPhWRN6t0Aib6GnuPQ/exec'); // Updated with actual API URL
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
    <h1 class="fade-in">🍽️ Canteen Menu</h1>
    <p v-if="menu.length === 0" class="loading">Loading menu...</p>
    <div v-else>
      <div v-for="category in categories" :key="category" class="category-section slide-in">
        <h2>{{ category }}</h2>
        <div class="menu-grid">
          <div v-for="(item, index) in menu.filter(i => i.category === category)" :key="index" class="menu-item zoom-in">
            <p class="item-name">{{ item.name }}</p>
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
  font-family: Arial, sans-serif;
}

.category-section {
  margin-bottom: 20px;
  background: #ffefdb;
  padding: 15px;
  border-radius: 8px;
  box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.1);
}

.menu-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
  justify-content: center;
}

.menu-item {
  border: 2px solid #ff9500;
  background: #ffe4b5;
  padding: 15px;
  border-radius: 10px;
  text-align: center;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.menu-item:hover {
  transform: scale(1.1);
  box-shadow: 4px 4px 15px rgba(0, 0, 0, 0.2);
}

.item-name {
  font-size: 1.2em;
  font-weight: bold;
}

.price {
  color: #c0392b;
  font-weight: bold;
  font-size: 1.1em;
}

.loading {
  font-size: 1.2em;
  color: #777;
}

/* Animations */
.fade-in {
  animation: fadeIn 1s ease-in-out;
}

.slide-in {
  animation: slideIn 1s ease-in-out;
}

.zoom-in {
  animation: zoomIn 0.5s ease-in-out;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideIn {
  from { transform: translateY(20px); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}

@keyframes zoomIn {
  from { transform: scale(0.8); opacity: 0; }
  to { transform: scale(1); opacity: 1; }
}

@media (max-width: 600px) {
  .canteen-container {
    padding: 10px;
  }
  .menu-grid {
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  }
  .menu-item {
    padding: 10px;
  }
}
</style> -->

<script setup>
import { ref, onMounted } from 'vue';

const menu = ref([]);
const categories = ref([]);
const isLoading = ref(true);
const isError = ref(false);

const fetchMenu = async () => {
  try {
    const response = await fetch('https://script.google.com/macros/s/AKfycbwZ-y4WExGhXtRXiZbEHyuYWFi5xeKSv3BN7xHzpXkNRjpURirNKPhWRN6t0Aib6GnuPQ/exec');
    if (!response.ok) throw new Error(`HTTP error! Status: ${response.status}`);
    
    const data = await response.json();
    menu.value = data;
    categories.value = [...new Set(data.map(item => item.category))];
  } catch (error) {
    console.error('Error fetching menu:', error);
    isError.value = true;
  } finally {
    isLoading.value = false;
  }
};

onMounted(fetchMenu);
</script>

<template>
  <div class="canteen-container">
    <h1 class="title">🍽️ Canteen Menu</h1>
    <p v-if="isLoading" class="loading">Loading menu...</p>
    <p v-if="isError" class="error">❌ Failed to load menu.</p>

    <div v-if="!isLoading && !isError">
      <div v-for="category in categories" :key="category" class="category-section">
        <h2 class="category-title">{{ category }}</h2>
        <div class="menu-grid">
          <div v-for="(item, index) in menu.filter(i => i.category === category)" :key="index" class="menu-item">
            <div class="food-img">
              <img :src="'https://source.unsplash.com/100x100/?' + item.name" :alt="item.name">
            </div>
            <div class="food-info">
              <p class="item-name">{{ item.item }}</p>
              <p class="price">₹{{ item.price }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Container */
.canteen-container {
  text-align: center;
  padding: 30px;
  background: url('https://source.unsplash.com/1600x900/?food,dining') center/cover;
  color: white;
  font-family: 'Poppins', sans-serif;
}

/* Title */
.title {
  font-size: 3em;
  font-weight: bold;
  text-shadow: 3px 3px 10px rgba(0, 0, 0, 0.5);
  margin-bottom: 20px;
}

/* Category Section */
.category-section {
  margin-bottom: 30px;
  padding: 20px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 15px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
}

/* Category Title */
.category-title {
  font-size: 2em;
  font-weight: bold;
  color: #ff9500;
  text-transform: uppercase;
  text-align: center;
  margin-bottom: 10px;
}

/* Menu Grid */
.menu-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 15px;
  padding: 10px;
}

/* Menu Item */
.menu-item {
  display: flex;
  align-items: center;
  background: rgba(0, 0, 0, 0.7);
  border-radius: 12px;
  padding: 15px;
  transition: transform 0.3s, box-shadow 0.3s;
}

.menu-item:hover {
  transform: scale(1.05);
  box-shadow: 0 6px 12px rgba(255, 165, 0, 0.6);
}

/* Food Image */
.food-img img {
  width: 80px;
  height: 80px;
  border-radius: 10px;
  object-fit: cover;
  margin-right: 15px;
  border: 2px solid #ffcc00;
}

/* Food Info */
.food-info {
  text-align: left;
}

.item-name {
  font-size: 1.4em;
  font-weight: bold;
  color: white;
}

.price {
  font-size: 1.3em;
  color: #ffcc00;
  font-weight: bold;
}

/* Loading and Error */
.loading, .error {
  font-size: 1.5em;
  font-weight: bold;
  color: #fff;
  margin-top: 20px;
}

/* Animations */
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(15px); }
  to { opacity: 1; transform: translateY(0); }
}

.category-section {
  animation: fadeIn 0.7s ease-in-out;
}

.menu-item {
  animation: fadeIn 1s ease-in-out;
}

/* Responsive Design */
@media (max-width: 768px) {
  .canteen-container {
    padding: 15px;
  }
  .menu-grid {
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  }
  .menu-item {
    flex-direction: column;
    text-align: center;
    padding: 10px;
  }
  .food-img img {
    margin-bottom: 10px;
  }
}
</style>
