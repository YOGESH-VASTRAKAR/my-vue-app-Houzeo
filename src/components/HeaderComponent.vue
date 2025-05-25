<!-- src/components/HeaderComponent.vue -->
<template>
  <div class="header-container">
    <!-- Mobile Header -->
    <div class="header mobile-header" v-if="isMobile">
      <div class="top-section">
        <div class="hamburger-menu">
          <button class="menu-button" @click="toggleMobileMenu">
            <img src="../assets/hamburger-menu.svg" alt="hamburger-menu" />
          </button>
        </div>

        <div class="logo">
          <img src="../assets/logo.svg" alt="Houzeo Logo" />
        </div>

        <div class="user-account">
          <button class="account-button">
            <img src="../assets/user-account.svg" alt="user-account" />
          </button>
        </div>
      </div>
      <div class="search-container">
        <div class="search-input-container">
          <div class="search-input">
            <input type="text" placeholder="Enter location" v-model="searchLocation" @focus="showLocationOptions = true" />
            <button class="clear-button" v-if="searchLocation" @click="clearSearch">×</button>
            <button class="search-button">
              <img src="../assets/search-button.svg" alt="search-button" />
            </button>

            <!-- Location Dropdown -->
            <div class="location-dropdown" v-if="showLocationOptions && !mobileMenuOpen">
              <div class="location-option" v-for="(loc, index) in filteredLocations" :key="index" @click="selectLocation(loc)">
                {{ loc }}
              </div>
            </div>
          </div>
          
          <!-- Filter button next to search input -->
          <button class="mobile-filter-button">
            <img src="../assets/filters-button.svg" alt="filters-button" />
          </button>
        </div>

        <div class="filter-options">
          <div class="filter-dropdown">
            <button class="dropdown-button" @click="toggleStatusDropdown">
              For Sale <i class="arrow-down"></i>
            </button>
            <div class="dropdown-menu" v-if="showStatusDropdown">
              <div class="dropdown-item" @click="selectStatus('For Sale')">For Sale</div>
              <div class="dropdown-item" @click="selectStatus('For Sold')">For Sold</div>
            </div>
          </div>
          <div class="filter-dropdown">
            <button class="dropdown-button">Price <i class="arrow-down"></i></button>
          </div>
          <button class="save-search-button">Save Search</button>
        </div>
      </div>
    </div>

    <!-- Desktop Header -->
    <div class="header desktop-header" v-else>
      <div class="top-header">
        <div class="logo">
          <img src="../assets/logo.svg" alt="Houzeo Logo" />
        </div>

        <div class="menu">
          <div class="dropdown">
            <button class="dropdown-button">Product <i class="arrow-down"></i></button>
          </div>
          <div class="dropdown">
            <button class="nav-button">How Houzeo Works</button>
          </div>
          <div class="dropdown">
            <button class="nav-button">Reviews</button>
          </div>          
          <div class="dropdown">
            <button class="dropdown-button">Buy <i class="arrow-down"></i></button>
          </div>
          <div class="dropdown">
            <button class="dropdown-button">Sell <i class="arrow-down"></i></button>
          </div>
          <div class="dropdown">
            <button class="nav-button">Pricing</button>
          </div>
          
          <div class="dropdown">
            <button class="dropdown-button">Resources <i class="arrow-down"></i></button>
          </div>
          
        </div>

        <div class="right-section">
        <button class="sign-in-button">Sign In</button>
          <div class="phone-number">
            (844) 448-0110
            <div class="business-hours">(8am to 8pm CST, Mon-Sat)</div>
          </div>
          
          <button class="listing-button">Start Free Listing</button>
        </div>
      </div>
      <div class="bottom-header">
        <div class="search-container">
          <div class="search-input">
            <input type="text" placeholder="Enter location" v-model="searchLocation" @focus="showLocationOptions = true" />
            <button class="clear-button" v-if="searchLocation" @click="clearSearch">×</button>
            <button class="search-button">
              <img src="../assets/search-button.svg" alt="search-button" />
            </button>

            <!-- Location Dropdown -->
            <div class="location-dropdown" v-if="showLocationOptions && !mobileMenuOpen">
              <div class="location-option" v-for="(loc, index) in filteredLocations" :key="index" @click="selectLocation(loc)">
                {{ loc }}
              </div>
            </div>
          </div>

          <div class="filter-options">
            <div class="filter-dropdown">
              <button class="dropdown-button" @click="toggleStatusDropdown">
              For Sale <i class="arrow-down"></i>
            </button>
            <div class="dropdown-menu" v-if="showStatusDropdown">
              <div class="dropdown-item" @click="selectStatus('For Sale')">For Sale</div>
              <div class="dropdown-item" @click="selectStatus('For Sold')">For Sold</div>
            </div>
            </div>
            <div class="filter-dropdown">
              <button class="dropdown-button">Price <i class="arrow-down"></i></button>
            </div>
            <div class="filter-dropdown">
              <button class="dropdown-button">Beds & Baths <i class="arrow-down"></i></button>
            </div>
            <div class="filter-dropdown">
              <button class="dropdown-button">Property Type <i class="arrow-down"></i></button>
            </div>
            <button class="filters-button">
              <img src="../assets/filters-button.svg" alt="filters-button" />
              Filters
            </button>
            <button class="saved-button">
              <img src="../assets/saved-button.svg" alt="saved-button" />
              Saved
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted, onUnmounted, computed } from 'vue';

export default defineComponent({
  name: 'HeaderComponent',
  setup() {
    const windowWidth = ref(window.innerWidth);
    const isMobile = ref(window.innerWidth < 768);
    const isSmallMobile = ref(window.innerWidth <= 418);
    const searchLocation = ref('Austin, TX');
    const mobileMenuOpen = ref(false);
    const showLocationOptions = ref(false);
    const showViewportInfo = ref(false);
    const showStatusDropdown = ref(false);
    const selectedStatus = ref('For Sale');

    const locations = [
      'Austin, TX',
      'Houston, TX',
      'Dallas, TX',
      'San Antonio, TX',
      'Fort Worth, TX',
      'El Paso, TX',
      'Arlington, TX'
    ];

    const statusOptions = [
      'For Sale',
      'Sold'
    ];

    const filteredLocations = computed(() => {
      if (!searchLocation.value) return locations;
      const query = searchLocation.value.toLowerCase();
      return locations.filter(loc => loc.toLowerCase().includes(query));
    });

    const handleResize = () => {
      windowWidth.value = window.innerWidth;
      isMobile.value = window.innerWidth < 768;
      isSmallMobile.value = window.innerWidth <= 418;
    };

    const clearSearch = () => {
      searchLocation.value = '';
      showLocationOptions.value = true;
    };

    const selectLocation = (location: string) => {
      searchLocation.value = location;
      showLocationOptions.value = false;
    };

    const toggleMobileMenu = () => {
      mobileMenuOpen.value = !mobileMenuOpen.value;
    };

    const toggleViewportIndicator = () => {
      showViewportInfo.value = !showViewportInfo.value;
    };

    const toggleStatusDropdown = () => {
      showStatusDropdown.value = !showStatusDropdown.value;
    };

    const selectStatus = (status: string) => {
      selectedStatus.value = status;
      showStatusDropdown.value = false;
    };

    const simulateDesktopView = () => {
      isMobile.value = false;
    };

    const simulateMobileView = () => {
      isMobile.value = true;
    };
    
    // Close dropdowns when clicked outside
    const handleClickOutside = (event: MouseEvent) => {
      const target = event.target as HTMLElement;
      
      if (!target.closest('.search-input')) {
        showLocationOptions.value = false;
      }
      
      if (!target.closest('.filter-dropdown')) {
        showStatusDropdown.value = false;
      }
    };

    onMounted(() => {
      window.addEventListener('resize', handleResize);
      document.addEventListener('click', handleClickOutside);
    });

    onUnmounted(() => {
      window.removeEventListener('resize', handleResize);
      document.removeEventListener('click', handleClickOutside);
    });

    return {
      isMobile,
      windowWidth,
      isSmallMobile,
      searchLocation,
      mobileMenuOpen,
      showLocationOptions,
      showViewportInfo,
      showStatusDropdown,
      selectedStatus,
      locations,
      statusOptions,
      filteredLocations,
      clearSearch,
      selectLocation,
      toggleMobileMenu,
      toggleViewportIndicator,
      toggleStatusDropdown,
      selectStatus,
      simulateDesktopView,
      simulateMobileView
    };
  }
});
</script>

<style scoped>
.header-container {
  width: 100%;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.1);
  background-color: white;
  position: sticky;
  top: 0;
  z-index: 1000;
}

/* Mobile Header Styles */
.mobile-header {
  padding: 10px 15px;
}

.top-section {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
}

.mobile-header .hamburger-menu,
.mobile-header .logo,
.mobile-header .user-account {
  display: flex;
  align-items: center;
}

.mobile-header .logo {
  flex: 1;
  justify-content: center;
}

.mobile-header .logo img {
  height: 28px;
}

.menu-button {
  background: none;
  border: none;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  padding: 5px;
}

.menu-button .bar {
  background-color: #333;
  height: 2px;
  width: 20px;
  margin: 2px 0;
}

.account-button {
  background: none;
  border: none;
  cursor: pointer;
  color: #333;
}

.search-input {
  display: flex;
  align-items: center;
  border: 1px solid #ccc;
  border-radius: 25px;
  padding: 2px 10px 2px 12px;
  position: relative;
  overflow: hidden;
  background-color: #fff;
  transition: border-color 0.3s ease; /* Transition for hover effect */
}

.search-input:hover,
.search-input:focus-within {
  border-color: #0B5AA5; /* Border color change on hover/focus */
}

.search-input input {
  flex: 1;
  border: none;
  outline: none;
  padding: 14px 4px;
  font-size: 16px;
  background: transparent;
}

.clear-button {
  background: none;
  border: none;
  color: #999;
  font-size: 20px;
  cursor: pointer;
  width: 25px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.search-button {
  background-color: #0B5AA5;
  border: none;
  border-radius: 50%;
  width: 36px;
  height: 36px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  margin-left: auto;
}

.filter-options {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
}

.filter-dropdown {
  flex: 1;
  min-width: 100px;
}

.filter-dropdown {
    position: relative;
  }

.dropdown-menu {
  position: absolute;
  top: 100%;
  left: 0;
  background: white;
  border: 1px solid #ddd;
  border-radius: 8px;
  min-width: 120px;
  z-index: 100;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  margin-top: 4px;
}

.dropdown-item {
  padding: 14px 16px;
  cursor: pointer;
  font-size: 14px;
  border-bottom: 1px solid #eee;
}

.dropdown-item:hover {
  background-color: #f8f8f8;
}

.dropdown-button {
  width: 100%;
  background-color: white;
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 14px 16px;
  font-size: 14px;
  text-align: left;
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  color: #333;
  font-weight: normal;
  transition: border-color 0.3s ease; /* Transition for hover effect */
}

.dropdown-button:hover {
  border-color:#074580; /* Border color change on hover */
}

.arrow-down {
  display: inline-block;
  width: 0;
  height: 0;
  border-left: 5px solid transparent;
  border-right: 5px solid transparent;
  border-top: 5px solid #333;
  margin-left: 10px;
}

.save-search-button {
  background-color: #0B5AA5;
  color: white;
  border: none;
  border-radius: 25px;
  padding: 14px 16px;
  flex: 1;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  transition: background-color 0.3s ease; /* Transition for hover effect */
}

.save-search-button:hover {
  background-color: #074580; /* Darker blue on hover */
}

/* Desktop Header Styles */
.desktop-header {
  padding: 0 20px;
}

.top-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 0;
  border-bottom: 1px solid #eee;
}

.top-header .logo img {
  height: 35px;
}

.menu {
  display: flex;
  gap: 20px;
  margin-left: 30px;
}

.nav-button {
  background: none;
  border: none;
  padding: 8px 0;
  font-size: 14px;
  color: #333;
  cursor: pointer;
  transition: color 0.3s ease; /* Transition for hover effect */
}

.nav-button:hover {
  color: #074580; /* Blue color on hover */
}

.menu .dropdown-button {
  background: none;
  border: none;
  border-radius: 0;
  padding: 8px 0;
  font-size: 14px;
  color: #333;
  transition: color 0.3s ease; /* Transition for hover effect */
}

.menu .dropdown-button:hover {
  color: #074580; /* Blue color on hover */
}

.right-section {
  display: flex;
  gap: 30px;
  align-items: center;
}

.phone-number {
  font-size: 14px;
}

.business-hours {
  font-size: 12px;
  color: #666;
  font-weight: normal;
}

.sign-in-button {
  background: none;
  border: none;
  font-size: 14px;
  cursor: pointer;
  transition: color 0.3s ease; /* Transition for hover effect */
}

.sign-in-button:hover {
  color: #074580; /* Blue color on hover */
}

.listing-button {
  background: linear-gradient(157deg, #2876C1, #2876C1);
  color: white;
  border: none;
  border-radius: 8px;
  padding: 14px 16px;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  transition: background-color 0.3s ease; /* Transition for hover effect */
}

.listing-button:hover {
  background-color: #0B5AA5; /* Darker blue on hover */
}

.bottom-header {
  padding: 15px 0;
}

.desktop-header .search-container {
  display: flex;
  align-items: center;
  gap: 15px;
}

.desktop-header .search-input {
  width: 280px;
  margin-bottom: 0;
}

.filter-options {
  flex: 1;
  display: flex;
  gap: 16px;
  align-items: center;
}

.filters-button{
  border-radius: 8px;
  padding: 14px 16px;
}

.saved-button{
  border-radius: 25px;
  padding: 14px 16px;
}

.filters-button, .saved-button {
  background: white;
  border: 1px solid #ccc;
  font-size: 14px;
  display: flex;
  align-items: center;
  gap: 12px;
  cursor: pointer;
  color: #333;
  transition: border-color 0.3s ease; /* Transition for hover effect */
}

.filters-button:hover, .saved-button:hover {
  border-color: #0B5AA5; /* Border color change on hover */
}

/* Responsive breakpoints */
@media (max-width: 767px) {
  .desktop-header {
    display: none;
  }
  /* New search container with filter button */
  .search-input-container {
    display: flex;
    align-items: center;
    width: 100%;
    gap: 8px;
    margin-bottom: 15px;
  }

  .mobile-filter-button {
    background: #ffffff;
    border: 1px solid #ccc;
    border-radius: 20px; /* Fully rounded corners */
    padding: 8px 10px;
    cursor: pointer;
    flex-shrink: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 40px; /* Match search input height */
    width: 40px; /* Make it a circle */
    box-shadow: 0 1px 2px rgba(0,0,0,0.05);
    transition: all 0.3s ease;
  }
  .mobile-filter-button:hover{
   border-color: #0B5AA5; 
  }

  .mobile-filter-button img {
    width: 20px;
    height: 20px;
  }

  .search-input {
    flex: 1;
    position: relative;
  }  
 
}

@media (min-width: 768px) {
  .mobile-header {
    display: none;
  }
  
}
/* Mobile-specific styles (420px and below) */
@media (max-width: 418px) {
  .mobile-header {
    padding: 8px 10px;
  }

  .top-section {
    margin-bottom: 10px;
  }

  .mobile-header .logo img {
    height: 24px;
  }

  .search-input-container {
    flex-direction: column;
    gap: 10px;
  }

  .search-input {
    width: 100%;
  }

  .mobile-filter-button {
    width: 100%;
    border-radius: 8px;
    height: 40px;
    justify-content: center;
  }

  .filter-options {
    flex-direction: column;
    gap: 8px;
  }

  .filter-dropdown {
    width: 100%;
  }

  .save-search-button {
    width: 100%;
  }

  .menu-button img,
  .account-button img {
    width: 20px;
    height: 20px;
  }
}
/* New styles for location dropdown */
.search-input {
  position: relative;
}

.location-dropdown {
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  background: white;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-top: 5px;
  max-height: 200px;
  overflow-y: auto;
  z-index: 1000;
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.1);
}

.location-option {
  padding: 10px 15px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.location-option:hover {
  background-color: #f5f5f5;
}

</style>
