<!-- src/App.vue -->
<template>
  <div id="app">
    <header-component />

    <!-- Mobile View -->
    <div v-if="isMobile" class="mobile-view">
      <div class="location-heading">
        <h1>Austin, TX real estate & homes for sale</h1>
        <div class="listings-info">
          <span>2,010 Homes</span>
          <div class="sort-options">
            <span>Sort by:</span>
            <select>
              <option>New Listing</option>
              <option>Price (Low to High)</option>
              <option>Price (High to Low)</option>
              <option>Beds</option>
              <option>Baths</option>
            </select>
          </div>
        </div>
      </div>
      <property-listings mobile-view />
      <div class="map-toggle">
        <button class="map-toggle-btn">
          <span>Map</span>
          <img src="./assets/map-toggle.svg" alt="map-toggle" />
        </button>
      </div>
    </div>

    <!-- Desktop View -->
    <div v-else class="desktop-view">
      <main class="main-content">
        <map-component class="map-section" />
        <div class="listings-container">
          <div class="location-heading">
            <h1>Austin, TX real estate & homes for sale</h1>
            <div class="listings-info">
              <span>2,010 Homes</span>
              <div class="sort-options">
                <span>Sort by:</span>
                <select>
                  <option>New Listing</option>
                  <option>Price (Low to High)</option>
                  <option>Price (High to Low)</option>
                  <option>Beds</option>
                  <option>Baths</option>
                </select>
              </div>
            </div>
          </div>
          <property-listings />
        </div>
      </main>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted, onUnmounted } from 'vue';
import HeaderComponent from './components/HeaderComponent.vue';
import MapComponent from './components/MapComponent.vue';
import PropertyListings from './components/PropertyListings.vue';

export default defineComponent({
  name: 'App',
  components: {
    HeaderComponent,
    MapComponent,
    PropertyListings
  },
  setup() {
    const isMobile = ref(window.innerWidth < 768);

    const handleResize = () => {
      isMobile.value = window.innerWidth < 768;
    };

    onMounted(() => {
      window.addEventListener('resize', handleResize);
    });

    onUnmounted(() => {
      window.removeEventListener('resize', handleResize);
    });

    return {
      isMobile
    };
  }
});
</script>

<style>
/* Main app styles */
body {
  margin: 0;
  background-color: #f5f5f5;
}

#app {
  width: 100%;
}

/* Mobile View Styles */
.mobile-view {
  padding: 0 15px;
}

.mobile-view .location-heading {
  padding: 15px 0;
}

.mobile-view .location-heading h1 {
  font-size: 18px;
  font-weight: 600;
  margin-bottom: 5px;
  margin-top: 0;
  color: #333;
}

.mobile-view .listings-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.mobile-view .listings-info span {
  font-size: 14px;
  color: #666;
}

.mobile-view .sort-options {
  display: flex;
  align-items: center;
  gap: 5px;
  font-size: 14px;
}

.mobile-view .sort-options span {
  color: #666;
}

.mobile-view .sort-options select {
  padding: 5px 10px;
  border: 1px solid transparent;
  border-radius: 4px;
  background-color: transparent;
  font-size: 14px;
  color: #0B5AA5;
  font-weight: 500;
}

.map-toggle {
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 100;
}

.map-toggle-btn {
  background-color: #0B5AA5;
  color: white;
  border: none;
  border-radius: 24px;
  padding: 14px 16px;
  font-size: 14px;
  font-weight: 500;
  display: flex;
  align-items: center;
  justify-content:center; 
  gap: 16px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

.map-toggle-btn svg {
  margin-top: 2px;
}
.map-toggle-btn:hover {
  background-color: #0055b3;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.25);
}

.map-toggle-btn:active {
  transform: translateY(0);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
}

/* Desktop View Styles */
.desktop-view .main-content {
  display: flex;
  height: calc(100vh - 150px);
}

.desktop-view .map-section {
  width: 50%;
  height: 100%;
  position: relative;
}

.desktop-view .listings-container {
  width: 50%;
  overflow-y: auto;
  padding: 0 15px;
}

.desktop-view .location-heading {
  padding: 15px 0;
  border-bottom: 1px solid #eee;
}

.desktop-view .location-heading h1 {
  font-size: 18px;
  font-weight: 600;
  margin: 0 0 10px 0;
  color: black;
}

.desktop-view .listings-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.desktop-view .listings-info span {
  font-size: 14px;
  color: #666;
}

.desktop-view .sort-options {
  display: flex;
  align-items: center;
  gap: 5px;
}

.desktop-view .sort-options span {
  font-size: 14px;
  color: #666;
}

.desktop-view .sort-options select {
  padding: 6px 12px;
  border: 1px solid transparent;
  border-radius: 4px;
  background-color: transparent;
  font-size: 14px;
  color: #0B5AA5;
  font-weight: 500;
}
.sort-options select:focus {
    outline: 1px solid transparent;
}

@media (max-width: 767px) {
  .desktop-view {
    display: none;
  }
}

@media (min-width: 768px) {
  .mobile-view {
    display: none;
  }
}
</style>