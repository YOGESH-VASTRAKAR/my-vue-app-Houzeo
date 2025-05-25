<template>
  <div :class="['property-listings', { 'mobile-listings': mobileView }]">
    <div class="property-card" v-for="(property, index) in properties" :key="index">
      <div class="property-image">
        
        <!-- Days on Houzeo tag -->
        <div class="days-tag">{{ property.daysListed }} days on Houzeo</div>

        <!-- Image slider -->
        <div class="image-slider" @mouseenter="showControls = true"    @mouseleave="showControls = false">
          <div class="slider-container">
            <div class="slider-track" :style="{ transform: `translateX(-${property.currentSlide * 100}%)` }">
            <div class="slide" v-for="(image, imgIndex) in property.images" :key="imgIndex">
              <img 
                :src="image" 
                :alt="`Property ${index + 1} Image ${imgIndex + 1}`" 
                loading="lazy"
                width="100%"
                height="100%"
                style="object-fit: cover"
              >
            </div>
          </div>

        <!-- Navigation arrows -->
        <button 
          class="slider-arrow prev" 
          v-show="showControls && property.images.length > 1 && property.showPrevButton"
          @click="goToSlide(property, property.currentSlide - 1)"
          aria-label="Previous image"
        >
          <img src="../assets/prev.svg" alt="prev" />
        </button>
        <button 
          class="slider-arrow next" 
          v-show="showControls && property.images.length > 1"
          @click="goToSlide(property, property.currentSlide + 1)"
          aria-label="Next image"
        >
        <img src="../assets/next.svg" alt="next" />          
        </button>

        <!-- Bullet points -->
        <div class="slider-controls" v-show="showControls || property.images.length > 1">
          <div class="slider-dots">
            <span
              v-for="(_, dotIndex) in property.images"
              :key="dotIndex"
              class="dot"
              :class="{ active: property.currentSlide === dotIndex }"
              @click="goToSlide(property, dotIndex)"
              :aria-label="`Go to image ${dotIndex + 1}`"
            ></span>
          </div>
        </div>
      </div>
    </div>


        <!-- Heart favorite button -->
        <button class="favorite-button" :class="{ 'favorited': property.favorited }" @click="toggleFavorite(property)">
          <img src="../assets/favorite button.svg" alt="favorite button" />
        </button>

        <!-- Powered by label (only show on some images) -->
        <div class="powered-label" v-if="index % 2 === 0">
            <img src="../assets/Powered by.svg" alt="Powered by" />
        </div>
      </div>

      <div class="property-info">

        <!-- Property type label -->
        <div class="property-type">
          <div class="property-type-decor">
            <span class="type-dot" :class="`type-${property.typeClass}`"></span>
            <span class="type-text">{{ property.type }}</span>
          </div>

          <!-- View count with eye icon -->
          <div class="view-count">
            <img src="../assets/eye icon.svg" alt="eye icon" />
            <span>{{ property.views }}K</span>
          </div>
        </div>
        <div class="price-section">

            <!-- Price info -->
                <div class="property-price">${{ formatPrice(property.price) }}</div>

                <!-- Property specs (beds, baths, sqft) -->
                <div class="property-specs">
                <div class="spec-item">
                    <span class="spec-value">{{ property.beds }}</span> Beds
                </div>
                <div class="spec-item">
                    <span class="spec-value">{{ property.baths }}</span> Baths
                </div>
                <div class="spec-item">
                    <span class="spec-value">{{ formatSqft(property.sqft) }}</span> sqft.
                </div>
                </div>
        </div>
        
        <!-- Address -->
        <div class="property-address"><span>{{ property.street }}</span>{{ property.address }}</div>

        <!-- Distribution info -->
        <div class="property-distribution">{{ property.distribution }}</div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

interface Property {
  id: number;
  images: string[];
  currentSlide: number;
  type: string;
  typeClass: string;
  price: number;
  beds: number;
  baths: number;
  sqft: number;
  street: string;
  address: string;
  daysListed: number;
  favorited: boolean;
  views: number;
  distribution: string;
  showPrevButton: boolean;
}

export default defineComponent({
  name: 'PropertyListings',
  props: {
    mobileView: {
      type: Boolean,
      default: false
    }
  },
  setup() {
    const properties = ref<Property[]>([
      {
        id: 1,
        images: [
          '1.png',
          '2.png',
          '3.png',
          '4.png',
          '1.png'
        ],
        currentSlide: 0,
        showPrevButton: false,
        type: 'House For Sale',
        typeClass: 'house',
        price: 3349000,
        beds: 4,
        baths: 3,
        sqft: 998,
        street:'2856 Meadow Park Ave, ',
        address: 'Henderson, NV 89052',
        daysListed: 6,
        favorited: false,
        views: 2.3,
        distribution: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID'
      },
      {
        id: 2,
        images: [  
          '2.png',
          '1.png',
          '3.png',
          '4.png',
          '2.png'
        ],
        currentSlide: 0,
        showPrevButton: false,
        type: 'Condo For Sale',
        typeClass: 'condo',
        price: 3349000,
        beds: 4,
        baths: 3,
        sqft: 998,
        street:'2856 Meadow Park Ave, ',
        address: 'Henderson, NV 89052',
        daysListed: 12,
        favorited: true,
        views: 2.3,
        distribution: 'Sotheby\'s International Realty'
      },
      {
        id: 3,
        images: [
          '2.png',
          '1.png',
          '3.png',
          '4.png',
          '2.png'
          ],
        currentSlide: 0,
        showPrevButton: false,
        type: 'Multi-family home For Sale',
        typeClass: 'multi',
        price: 3349000,
        beds: 4,
        baths: 3,
        sqft: 998,
        street:'2856 Meadow Park Ave, ',
        address: 'Henderson, NV 89052',
        daysListed: 17,
        favorited: false,
        views: 2.3,
        distribution: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID'
      },
      {
        id: 4,
        images: [
          '4.png',
          '1.png',
          '3.png',
          '2.png',
          '4.png'
        ],
        currentSlide: 0,
        showPrevButton: false,
        type: 'House For Sale',
        typeClass: 'house',
        price: 3349000,
        beds: 4,
        baths: 3,
        sqft: 998,
        street:'2856 Meadow Park Ave, ',
        address: 'Henderson, NV 89052',
        daysListed: 10,
        favorited: false,
        views: 2.3,
        distribution: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID'
      },
      {
        id: 5,
        images: [
          '4.png',
          '1.png',
          '3.png',
          '2.png',
          '4.png'
        ],
        currentSlide: 0,
        showPrevButton: false,
        type: 'House For Sale',
        typeClass: 'house',
        price: 3349000,
        beds: 4,
        baths: 3,
        sqft: 998,
        street:'2856 Meadow Park Ave, ',
        address: 'Henderson, NV 89052',
        daysListed: 10,
        favorited: false,
        views: 2.3,
        distribution: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID'
      },
      {
        id: 6,
        images: [
          '3.png',
          '1.png',
          '4.png',
          '2.png',
          '3.png'
        ],
        currentSlide: 0,
        showPrevButton: false,
        type: 'House For Sale',
        typeClass: 'house',
        price: 3349000,
        beds: 4,
        baths: 3,
        sqft: 998,
        street:'2856 Meadow Park Ave, ',
        address: 'Henderson, NV 89052',
        daysListed: 10,
        favorited: false,
        views: 2.3,
        distribution: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID'
      }
    ]);

    const showControls = ref(false);

    const goToSlide = (property: Property, slideIndex: number) => {
      // Handle wrap-around for next/previous buttons
      if (slideIndex < 0) {
        slideIndex = property.images.length - 1;
      } else if (slideIndex >= property.images.length) {
        slideIndex = 0;
      }

      // Show previous button if we're not on the first slide
      if (slideIndex > 0) {
        property.showPrevButton = true;
      }
      property.currentSlide = slideIndex;
    };

    const nextSlide = (property: Property) => {
      goToSlide(property, property.currentSlide + 1);
    };

    const prevSlide = (property: Property) => {
      goToSlide(property, property.currentSlide - 1);
    };

    const toggleFavorite = (property: Property) => {
      property.favorited = !property.favorited;
    };

    const formatPrice = (price: number) => {
      return price.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    };

    const formatSqft = (sqft: number) => {
      return sqft.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    };

    return {
      properties,
      showControls,
      goToSlide,
      nextSlide,
      prevSlide,
      toggleFavorite,
      formatPrice,
      formatSqft
    };
  }
});
</script>

<style scoped>

/* Image slider styling */
.image-slider {
  position: relative;
  height: 100%;
  overflow: hidden;
  border-radius: 8px;
}

.slider-container {
  position: relative;
  width: 100%;
  height: 100%;
}

.slider-track {
  display: flex;
  height: 100%;
  transition: transform 0.3s ease-in-out;
}

.slide {
  flex: 0 0 100%;
  height: 100%;
  position: relative;
}

/* Navigation arrows */
.slider-arrow {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 40px;
  height: 40px;
  background-color: white;
  border: none;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  z-index: 2;
  transition: background-color 0.2s ease;
}

.slider-arrow:hover {
  background-color:rgba(241, 236, 236, 0.86);
}

.slider-arrow.prev {
  left: 10px;
}

.slider-arrow.next {
  right: 10px;
}

.slider-arrow svg {
  width: 24px;
  height: 24px;
}

/* Bullet points */
.slider-controls {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 10px;
  display: flex;
  justify-content: center;
  z-index: 2;
}

.slider-dots {
  display: flex;
  gap: 5px;
}

.dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background-color: rgba(255, 255, 255, 0.5);
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.dot.active {
  background-color: white;
}

.dot:hover {
  background-color: rgba(255, 255, 255, 0.8);
}

.property-listings {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  padding: 15px 0;
}

.mobile-listings {
  grid-template-columns: 1fr;
}

.property-card {
  background-color: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  transition: box-shadow 0.3s ease;
}

.property-card:hover {
  box-shadow: 0 3px 8px rgba(0, 0, 0, 0.15);
}

.property-image {
  position: relative;
  height: 220px;
}

.price-section{
  display: flex;
  align-items: center;
  justify-content: space-between;
}

/* Days tag styling */
.days-tag {
  position: absolute;
  top: 10px;
  left: 10px;
  background-color:white;
  color: black;
  padding: 5px 10px;
  border-radius: 24px;
  font-size: 12px;
  font-weight: 500;
  z-index: 2;
}

/* Favorite button styling */
.favorite-button {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: transparent;
  border: none;
  border-radius: 50%;
  width: 36px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  z-index: 2;
  color: #666;
  transition: transform 0.2s ease, color 0.2s ease;
}

.favorite-button:hover {
  transform: scale(1.1);
  animation: pulse 1s infinite;
}

.favorite-button.favorited {
  color: #ff4d4d;
}

.favorite-button.favorited svg {
  fill: #ff4d4d;
}

@keyframes pulse {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.1);
  }
  100% {
    transform: scale(1);
  }
}

/* Powered by label styling */
.powered-label {
  position: absolute;
  bottom: 10px;
  right: 10px;
  z-index: 2;
}

/* Property info styling */
.property-info {
  padding: 15px;
}

.property-type {
  display: flex;
  align-items: center;
  margin-bottom: 8px;
  justify-content: space-between;
}
.property-type-decor{
  border: 1px solid #0000001A;
  padding: 5px 10px;
  border-radius: 24px;
  font-size: 12px;
}
.type-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  display: inline-block;
  margin-right: 6px;
}

.type-house {
  background-color: #00B67B;
}

.type-condo {
  background-color: #00B67B;
}

.type-multi {
  background-color: #00B67B;
}

.type-text {
  font-size: 13px;
  color: #000000;
}

.view-count {
  display: flex;
  align-items: center;
  gap: 5px;
  color: #000000;
  font-size: 13px;
}

.view-count svg {
  width: 16px;
  height: 16px;
  stroke-width: 1.5;
}

.property-price {
  font-size: 22px;
  font-weight: 600;
  color: #0B5AA5;
  margin-bottom: 8px;
}

.property-specs {
  display: flex;
  gap: 4px;
  margin-bottom: 10px;
  font-size: 14px;
  color: #666;
}

.spec-item {
  display: flex;
  align-items: center;
  gap: 4px;
}

.spec-value {
  font-weight: 600;
  color: #0B5AA5;
}

.property-address {
  font-size: 13px;
  color: #333;
  margin-bottom: 6px;
}
.property-address span{
  font-weight: 500;
}
.property-distribution {
  font-size: 12px;
  color: #00000080;
}

/* Responsive styles */
@media (max-width: 767px) {
  .property-listings {
    grid-template-columns: 1fr;
    padding: 10px 0;
    gap: 15px;
  }

  .property-image {
    height: 280px;
  }

  .property-price {
    font-size: 20px;
  }

  .property-specs {
    gap: 10px;
    font-size: 13px;
  }

  .property-address {
    font-size: 12px;
  }
}

/* iPhone 14 Pro Max specific styles */
@media only screen
  and (device-width: 430px)
  and (device-height: 932px)
  and (-webkit-device-pixel-ratio: 3) {
  .property-image {
    height: 220px;
  }
}
</style>
