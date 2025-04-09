<!-- 
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
</style> -->
<!-- **************************************************************************************************************************************************************** -->

<!-- 
<script setup>
import { ref, onMounted } from 'vue';

const menu = ref([]);
const categories = ref([]);

const getImageUrl = (fileName) => {
  try {
    return new URL(`./assets/menu_images/${fileName}`, import.meta.url).href;
  } catch (e) {
    console.warn(`Image not found: ${fileName}`);
    return '';
  }
};

const fetchMenu = async () => {
  try {
    const response = await fetch('https://script.google.com/macros/s/AKfycbwZ-y4WExGhXtRXiZbEHyuYWFi5xeKSv3BN7xHzpXkNRjpURirNKPhWRN6t0Aib6GnuPQ/exec');
    const data = await response.json();

    menu.value = data.map(item => ({
      ...item,
      imageUrl: getImageUrl(item.item_image)
    }));

    categories.value = [...new Set(data.map(item => item.category))];
  } catch (error) {
    console.error('Error fetching menu:', error);
  }
};

onMounted(fetchMenu);
</script>

<template>
  <div class="canteen-container">
    <h1 class="title">🍴 Welcome to the AIAT Canteen</h1>
    <p v-if="menu.length === 0" class="loading">Loading the delicious menu...</p>
    <div v-else>
      <div v-for="category in categories" :key="category" class="category-section fade-in">
        <h2 class="category-title">{{ category }}</h2>
        <div class="menu-list">
          <div
            v-for="(item, index) in menu.filter(i => i.category === category)"
            :key="index"
            class="menu-item slide-in"
          >
            <img :src="item.imageUrl" :alt="item.item" class="item-image" />
            <div class="item-details">
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
.canteen-container {
  padding: 30px;
  background: linear-gradient(135deg, #ff9a9e, #fad0c4, #fad0c4);
  color: #fff;
  font-family: 'Poppins', sans-serif;
  min-height: 100vh;
  background-size: cover;
  background-position: center;
  box-sizing: border-box;
}

.title {
  font-size: 2.8em;
  font-weight: bold;
  margin-bottom: 30px;
  text-shadow: 2px 2px 8px rgba(0, 0, 0, 0.3);
  text-align: center;
  color: #fff;
}

.category-section {
  margin-bottom: 30px;
  padding: 15px;
  background: rgba(0, 0, 0, 0.2);
  border-radius: 15px;
}

.category-title {
  font-size: 2em;
  font-weight: 700;
  color: #fff700;
  margin-bottom: 20px;
  padding-left: 10px;
  text-shadow: 1px 1px #000;
}

.menu-list {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.menu-item {
  display: flex;
  align-items: center;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 12px;
  padding: 15px;
  gap: 15px;
  box-shadow: 0 0 10px rgba(255, 255, 255, 0.2);
  transition: transform 0.3s ease;
}

.menu-item:hover {
  transform: scale(1.02);
}

.item-image {
  width: 80px;
  height: 80px;
  object-fit: cover;
  border-radius: 12px;
  border: 3px solid #fff;
}

.item-details {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.item-name {
  font-size: 1.4em;
  font-weight: bold;
  color: #fff;
}

.price {
  font-size: 1.2em;
  color: #00ffcc;
  font-weight: 600;
}

.loading {
  font-size: 1.5em;
  color: #ffffffcc;
  text-align: center;
  margin-top: 50px;
}

/* Animations */
.fade-in {
  animation: fadeIn 1s ease-in-out;
}

.slide-in {
  animation: slideIn 0.7s ease-in-out;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideIn {
  from { transform: translateY(30px); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}

/* Responsive */
@media (max-width: 768px) {
  .menu-item {
    flex-direction: column;
    align-items: flex-start;
  }

  .item-image {
    width: 100%;
    height: auto;
  }

  .item-name, .price {
    font-size: 1.2em;
  }

  .title {
    font-size: 2em;
  }
}
</style>
 -->

 <script setup>
import { ref, onMounted } from 'vue';

const menu = ref([]);
const categories = ref([]);

const getImageUrl = (fileName) => {
  try {
    return new URL(`./assets/menu_images/${fileName}`, import.meta.url).href;
  } catch (e) {
    console.warn(`Image not found: ${fileName}`);
    return '';
  }
};

const fetchMenu = async () => {
  try {
    const response = await fetch('https://script.google.com/macros/s/AKfycbwZ-y4WExGhXtRXiZbEHyuYWFi5xeKSv3BN7xHzpXkNRjpURirNKPhWRN6t0Aib6GnuPQ/exec');
    const data = await response.json();
    menu.value = data.map(item => ({
      ...item,
      imageUrl: getImageUrl(item.item_image)
    }));
    categories.value = [...new Set(data.map(item => item.category))];
  } catch (error) {
    console.error('Error fetching menu:', error);
  }
};

onMounted(fetchMenu);
</script>

<template>
  <div class="canteen-container">
    <h1 class="title"> CHAI & CHILL Canteen Menu</h1>
    <p v-if="menu.length === 0" class="loading">Loading deliciousness...</p>
    <div v-else>
      <div v-for="category in categories" :key="category" class="category-section fade-in">
        <h2 class="category-title">{{ category }}</h2>
        <div class="menu-grid">
          <div
            v-for="(item, index) in menu.filter(i => i.category === category)"
            :key="index"
            class="menu-card slide-in"
          >
            <img :src="item.imageUrl" :alt="item.item" class="item-image" />
            <div class="item-info">
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
@import url('https://fonts.googleapis.com/css2?family=Nunito:wght@400;700&display=swap');

.canteen-container {
  font-family: 'Nunito', sans-serif;
  background: linear-gradient(to right, #ff914d, #6b4226);
  color: #fff;
  min-height: 100vh;
  padding: 30px 20px;
  box-sizing: border-box;
}

.title {
  font-size: 2.5rem;
  text-align: center;
  margin-bottom: 30px;
  color: #fff7e6;
  text-shadow: 1px 1px 4px #000;
}

.loading {
  text-align: center;
  font-size: 1.5rem;
  color: #ffe8d6;
}

.category-section {
  margin-bottom: 40px;
}

.category-title {
  font-size: 1.8rem;
  margin-bottom: 20px;
  color: #ffcf80;
  border-left: 6px solid #fff;
  padding-left: 12px;
}

.menu-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 20px;
}

.menu-card {
  background: #3e2c23;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
  transition: transform 0.3s ease;
}

.menu-card:hover {
  transform: scale(1.03);
}

.item-image {
  width: 100%;
  height: 160px;
  object-fit: cover;
}

.item-info {
  padding: 15px;
  text-align: center;
}

.item-name {
  font-size: 1.2rem;
  font-weight: 700;
  color: #ffa94d;
  margin-bottom: 5px;
}

.price {
  font-size: 1rem;
  font-weight: 600;
  color: #ffdd99;
}

/* Animations */
.fade-in {
  animation: fadeIn 0.8s ease-in-out;
}

.slide-in {
  animation: slideIn 0.6s ease-in-out;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideIn {
  from { transform: translateY(20px); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}

/* Responsive */
@media (max-width: 480px) {
  .title {
    font-size: 2rem;
  }

  .category-title {
    font-size: 1.4rem;
  }

  .item-name {
    font-size: 1rem;
  }

  .price {
    font-size: 0.95rem;
  }
}
</style>
