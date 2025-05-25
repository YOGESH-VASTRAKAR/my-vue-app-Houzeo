<template>
  <div class="map-container">
    <div class="google-map">
      <!-- Google Maps style map -->
     
     
<iframe class="map-image" src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d20600912.710447516!2d-107.78261849678184!3d41.4485443680034!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x8644b599a0cc032f%3A0x5d9b464bd469d57a!2sAustin%2C%20TX%2C%20USA!5e0!3m2!1sen!2sin!4v1748008838087!5m2!1sen!2sin" width="600" height="450" style="border:0;" allow="fullscreen" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>
      <!-- Property markers -->
      <div class="map-markers">
        <div class="map-marker" v-for="(marker, index) in markers" :key="index"
            :style="{ left: marker.left + '%', top: marker.top + '%' }">
          <div class="marker-pin">
            <img src="../assets/marker-pin.svg" alt="marker-pin" />
          </div>
        </div>

        <!-- Cluster markers for areas with many properties -->
        <div class="cluster-marker" v-for="(cluster, index) in clusters" :key="'cluster-'+index"
            :style="{ left: cluster.left + '%', top: cluster.top + '%' }">
          <div class="cluster-pin">
            {{ cluster.count }}
          </div>
        </div>
      </div>
    </div>

    
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'MapComponent',
  setup() {
    // Sample marker data
    const markers = ref([
      { left: 35, top: 30 },
      { left: 40, top: 20 },
      { left: 60, top: 40 },
      { left: 65, top: 25 },
      { left: 30, top: 50 },
      { left: 30, top: 30 },
      { left: 25, top: 40 },
      { left: 35, top: 25 },
      { left: 40, top: 50 },
      { left: 45, top: 60 },
      { left: 28, top: 25 },
      { left: 32, top: 50 },
      { left: 42, top: 47 },
      { left: 41, top: 52 },
      { left: 38, top: 25 },
      { left: 36, top: 35 },
      { left: 48, top: 45 },
      { left: 68, top: 40 },
      { left: 65, top: 42 },
      { left: 64, top: 44 },
      { left: 63, top: 41 },
      { left: 33, top: 34 },
      { left: 35, top: 30 },
      { left: 35, top: 36 },
      { left: 35, top: 51 },
      { left: 33, top: 53 },
      { left: 35, top: 49 },
      { left: 35, top: 48 },
      { left: 40, top: 60 },
      { left: 43, top: 60 },
      { left: 36, top: 59 },
      { left: 39, top: 62 },
      { left: 37, top: 63 },
      { left: 69, top: 32 },
      { left: 71, top: 34 },
      { left: 73, top: 31 },
      { left: 75, top: 35 },
      { left: 69, top: 42 },
      { left: 71, top: 41 },
      { left: 73, top: 44 },
      { left: 75, top: 45 },
    ]);

    // Sample cluster data
    const clusters = ref([
      { left: 38, top: 25, count: 28 },
      { left: 35, top: 35, count: 16 },
      { left: 70, top: 40, count: 12 },
      { left: 38, top: 61, count: 24 },
      { left: 35, top: 53, count: 15 },
      { left: 70, top: 30, count: 14 },
    ]);

    return {
      markers,
      clusters
    };
  }
});
</script>

<style scoped>
.map-container {
  width: 100%;
  height: 100%;
  position: relative;
  overflow: hidden;
}

.google-map {
  width: 100%;
  height: 100%;
  position: relative;
}

.map-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.map-markers {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
}

.map-marker {
  position: absolute;
  transform: translate(-50%, -100%);
  z-index: 1;
}

.marker-pin {
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  pointer-events: auto;
}

.cluster-marker {
  position: absolute;
  transform: translate(-50%, -50%);
  z-index: 2;
}

.cluster-pin {
  width: 36px;
  height: 36px;
  background-color: #0B5AA5;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  font-weight: bold;
  font-size: 14px;
  cursor: pointer;
  pointer-events: auto;
  border: 2px solid white;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.map-controls {
  position: absolute;
  bottom: 20px;
  right: 20px;
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 10px;
}

.zoom-controls {
  display: flex;
  flex-direction: column;
  background: white;
  border-radius: 2px;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.2);
  overflow: hidden;
}

.zoom-btn {
  width: 40px;
  height: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 20px;
  font-weight: bold;
  border: none;
  background: white;
  color: #666;
  cursor: pointer;
  border-bottom: 1px solid #eee;
}

.zoom-btn:last-child {
  border-bottom: none;
}

.zoom-btn:hover {
  background-color: #f5f5f5;
}

.map-attribution {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  font-size: 10px;
  color: #666;
}

.keyboard-text {
  text-decoration: underline;
  margin-bottom: 2px;
}

.map-data {
  font-size: 8px;
}

@media (max-width: 767px) {
  .zoom-controls {
    display: none;
  }

  .map-attribution {
    display: none;
  }
}
</style>
