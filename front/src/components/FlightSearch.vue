<template>
  <div class="container">
    <!-- Background Section -->
    <div class="background-image">
      <div class="content-wrapper">
        <!-- Navigation Tabs -->
        <div class="tabs">
          <button class="tab active">
            <i class="fas fa-plane"></i> Flight
          </button>
          <button class="tab">
            <i class="fas fa-globe"></i> Tour
          </button>
        </div>

        <!-- Travel Type Selection -->
        <div class="travel-types">
          <label><input type="radio" name="travelType" value="oneWay" v-model="travelType" /> One Way</label>
          <label><input type="radio" name="travelType" value="roundTrip" v-model="travelType" /> Round Trip</label>
          <label><input type="radio" name="travelType" value="multiCity" v-model="travelType" /> Multi City</label>
        </div>

        <!-- Flight Search Inputs -->
        <div class="form-grid">
          <div class="form-group" v-if="travelType === 'oneWay' || travelType === 'roundTrip' || travelType === 'multiCity'">
            <label>From</label>
            <div class="input-with-icon">
              <i class="fas fa-plane-departure"></i>
              <input
                type="text"
                placeholder="Search Departure Location"
                v-model="searchFrom"
                @input="filterLocations('from')"
              />
              <ul v-if="filteredFrom.length > 0" class="dropdown">
                <li v-for="(location, index) in filteredFrom" :key="index" @click="selectLocation('from', location)">
                  {{ location }}
                </li>
              </ul>
            </div>
          </div>

          <div class="form-group" v-if="travelType === 'oneWay' || travelType === 'roundTrip' || travelType === 'multiCity'">
            <label>To</label>
            <div class="input-with-icon">
              <i class="fas fa-plane-arrival"></i>
              <input
                type="text"
                placeholder="Search Destination Location"
                v-model="searchTo"
                @input="filterLocations('to')"
              />
              <ul v-if="filteredTo.length > 0" class="dropdown">
                <li v-for="(location, index) in filteredTo" :key="index" @click="selectLocation('to', location)">
                  {{ location }}
                </li>
              </ul>
            </div>
          </div>

          <div class="form-group" v-if="travelType === 'oneWay' || travelType === 'roundTrip' || travelType === 'multiCity'">
            <label>Date</label>
            <div class="input-with-icon">
              <i class="fas fa-calendar-alt"></i>
              <input type="date" v-model="date" />
            </div>
          </div>

          <!-- For Round Trip, show additional fields -->
          <div class="form-group" v-if="travelType === 'roundTrip'">
            <label>Return Date</label>
            <div class="input-with-icon">
              <i class="fas fa-calendar-alt"></i>
              <input type="date" v-model="returnDate" />
            </div>
          </div>

          <div class="form-group" v-if="travelType === 'oneWay' || travelType === 'roundTrip' || travelType === 'multiCity'">
            <label>Person</label>
            <div class="input-with-icon">
              <i class="fas fa-user"></i>
              <select v-model="personCount">
                <option>1 Economy</option>
                <option>2 Economy</option>
                <option>3 Economy</option>
              </select>
            </div>
          </div>

          <!-- For Multi City, show additional fields -->
          <div class="form-group" v-if="travelType === 'multiCity'">
            <label>City 2</label>
            <div v-for="(city, index) in cities" :key="index" class="input-with-icon">
              <i class="fas fa-plane"></i>
              <input
                type="text"
                :placeholder="'Search Location ' + (index + 2)"
                v-model="city.search"
                @input="filterLocations('city' + (index + 2))"
              />
              <ul v-if="getFilteredCities(index).length > 0" class="dropdown">
                <li v-for="(location, idx) in getFilteredCities(index)" :key="idx" @click="selectCity(index, location)">
                  {{ location }}
                </li>
              </ul>
              <button v-if="index > 1" @click="removeCity(index)">Remove City</button>
            </div>
            <button v-if="cities.length < 5" @click="addCity">Add Another City</button>
          </div>

          <div class="form-group full-width">
            <button class="search-btn" @click="handleSearch">Search Flight</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: "FlightSearch",
  data() {
    return {
      searchFrom: "",
      searchTo: "",
      travelType: "oneWay", // Default set to one way
      date: "",
      returnDate: "",
      personCount: "1 Economy",
      locations: [], // All flight locations worldwide
      filteredFrom: [],
      filteredTo: [],
      cities: [{ search: "" }], // Array to store dynamic cities
    };
  },
  methods: {
    // Filter locations based on search input
    filterLocations(type) {
      if (type === "from") {
        this.filteredFrom = this.locations.filter((location) =>
          location.toLowerCase().includes(this.searchFrom.toLowerCase())
        );
      } else if (type === "to") {
        this.filteredTo = this.locations.filter((location) =>
          location.toLowerCase().includes(this.searchTo.toLowerCase())
        );
      } else if (type.startsWith("city")) {
        const cityIndex = parseInt(type.replace("city", "")) - 2;
        if (this.cities[cityIndex]) {
          this.cities[cityIndex].filtered = this.locations.filter((location) =>
            location.toLowerCase().includes(this.cities[cityIndex].search.toLowerCase())
          );
        }
      }
    },
    // Select location and set it in the respective field
    selectLocation(type, location) {
      if (type === "from") {
        this.searchFrom = location;
        this.filteredFrom = [];
      } else if (type === "to") {
        this.searchTo = location;
        this.filteredTo = [];
      }
    },
    selectCity(index, location) {
      this.cities[index].search = location;
      this.cities[index].filtered = [];
    },
    // Add another city to the multi city list
    addCity() {
      this.cities.push({ search: "" });
    },
    // Remove a city from the multi city list
    removeCity(index) {
      if (this.cities.length > 1) {
        this.cities.splice(index, 1);
      }
    },
    // Get the filtered cities for a specific city input field
    getFilteredCities(index) {
      return this.cities[index]?.filtered || [];
    },
    // Fetch flight locations when the component loads
    async fetchLocations() {
      try {
        // Fetching all flight destinations (for simplicity, use a mock API or static JSON)
        const response = await fetch(
          "https://api.example.com/all-flight-locations"
        );
        this.locations = await response.json();
      } catch (error) {
        console.error("Error fetching locations:", error);
      }
    },
    // Handle the Search Flight action
    handleSearch() {
      // Check if all the required fields are filled
      if (!this.searchFrom || !this.searchTo || !this.date || !this.personCount) {
        alert("Please fill in all the required details!");
        return;
      }

      // For round trip, check return date
      if (this.travelType === "roundTrip" && !this.returnDate) {
        alert("Please select a return date!");
        return;
      }

      // For multi city, ensure there are at least 2 cities filled
      if (this.travelType === "multiCity") {
        const filledCities = this.cities.filter(city => city.search !== "").length;
        if (filledCities < 2) {
          alert("Please select at least 2 cities for your multi-city trip!");
          return;
        }
      }

      // Proceed with the search (For now, just log the data)
      console.log("Searching for flights...");
      // You can call your search API here or trigger further actions
    }
  },
  mounted() {
    // Fetch locations when the component is mounted
    this.fetchLocations();
  }
};
</script>




<style scoped>
/* Global Styling */
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background-color: #f8f9fa; /* Soft background to contrast the form */
}

/* Container and Background */
.container {
  min-height: 100vh; /* Ensures it spans the viewport */
  width: 100%; /* Ensures it spans the full width of the viewport */
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  position: relative; /* For layered positioning */
  overflow: hidden; /* Prevent any unwanted overflow */
}

/* Image inside the container */
.container img {
  width: 100%; /* Ensures the image takes full width */
  height: auto; /* Maintain aspect ratio */
}


.background-image {
  width: 100%;
  height: 65%;
  display: flex;
  justify-content: center;
  align-items: flex-end;
  position: absolute;
  top: 0;
  z-index: 1;

  /* Background Settings */
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  
  /* Ensure no default background (no white screen) */
  background-image: url('/images/mountainn.png'), 
                    url('/images/plane.jpg'), 
                    url('/images/city.jpg'), 
                    url('/images/dark.jpg');

  /* Animation */
  animation: backgroundAnimation 16s infinite;
}

@keyframes backgroundAnimation {
  0%, 100% {
    background-image: url('/images/mountainn.png'), 
                      url('/images/plane.jpg'), 
                      url('/images/city.jpg'), 
                      url('/images/dark.jpg');
  }
  25% {
    background-image: url('/images/plane.jpg'), 
                      url('/images/mountainn.png'), 
                      url('/images/city.jpg'), 
                      url('/images/dark.jpg');
  }
  50% {
    background-image: url('/images/city.jpg'), 
                      url('/images/plane.jpg'), 
                      url('/images/mountainn.png'), 
                      url('/images/dark.jpg');
  }
  75% {
    background-image: url('/images/dark.jpg'), 
                      url('/images/plane.jpg'), 
                      url('/images/city.jpg'), 
                      url('/images/mountainn.png');
  }
}


.content-wrapper {
  background-color: white;
  padding: 20px;
  border-radius: 16px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  width: 90%;
  max-width: 800px;
  transform: translateY(30%); /* Centers it over the background */
  z-index: 2; /* Ensures it’s above the background image */
  position: relative; /* Allows layering */
}

/* Tabs */
.tabs {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.tab {
  padding: 10px 20px;
  border: none;
  background: none;
  cursor: pointer;
  font-size: 18px;
  font-weight: bold;
  color: #777;
  transition: color 0.2s, border-bottom 0.2s;
}

.tab.active {
  color: #007bff;
  border-bottom: 2px solid #007bff;
}

.tab:hover {
  color: #0056b3;
}

/* Travel Type Selection */
.travel-types {
  display: flex;
  margin-top: 45px;
  justify-content: center;
  gap: 20px;
  margin-bottom: 30px;
}

.travel-types label {
  font-size: 18px;
  color: #333;
  cursor: pointer;
}

/* Form Grid */
.form-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: 20px;
  align-items: center;
}

/* Form Group */
.form-group {
  display: flex;
  flex-direction: column;
}

.form-group label {
  font-size: 16px;
  margin-bottom: 8px;
  color: #555;
}

.input-with-icon {
  position: relative;
  width: 100%;
}

.input-with-icon i {
  position: absolute;
  top: 50%;
  left: 12px;
  transform: translateY(-50%); /* Adjust the vertical alignment */
  color: #000; /* Ensure the icon color is dark */
  font-size: 16px;
}

/* Adjust for Icon Placement in Inputs */
.input-with-icon select,
.input-with-icon input {
  width: 100%;
  padding: 10px 10px 10px 40px; /* Adjust for left icon */
  border: 1px solid #ddd;
  border-radius: 8px;
  font-size: 14px;
  box-sizing: border-box;
}

/* Full Width Button Group */
.form-group.full-width {
  grid-column: span 4;
  display: flex;
  justify-content: center;
  align-items: center;
}

/* Search Button */
.search-btn {
  width: 20%;
  padding: 12px;
  background-color: #007bff;
  color: white;
  font-size: 16px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.search-btn:hover {
  background-color: #0056b3;
}

/* Default PC Layout - No Changes Here */

/* Global Styles */
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background-color: #f8f9fa;
}

/* Mobile Optimization */
@media (max-width: 768px) {
  /* Background image positioning */
  .background-image {
    position: absolute;
    top: 0;  /* Set top to 0 so it stays at the top of the screen */
    left: 0;
    width: 100%;
    height: 70vh;  /* Set height to 50% of the viewport height */
    background-size: cover;
    background-position: center;
  }

  /* Content wrapper (move closer to the top) */
  .content-wrapper {
    position: relative;
    background-color: white;
    padding: 20px;
    border-radius: 16px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    width: 90%;
    max-width: 800px;
    margin-top: 10px; /* Reduced margin to bring content up */
  }

  /* Tabs (Flight/Tour) */
  .tabs {
    display: flex;
    justify-content: center;
    margin-bottom: 20px;
    gap: 10px;
    z-index: 2; /* Ensure it's above other content */
  }

  .tab {
    padding: 10px 20px;
    background: none;
    border: none;
    font-size: 16px;
    font-weight: bold;
    color: #555;
    cursor: pointer;
    border-radius: 8px;
    transition: color 0.2s, border-bottom 0.2s;
  }

  .tab.active {
    color: #007bff;
    border-bottom: 2px solid #007bff;
  }

  .tab:hover {
    color: #0056b3;
  }

  /* Travel Type selection */
  .travel-types {
    display: flex;
    justify-content: center;
    gap: 15px;
    margin-bottom: 20px;
    flex-wrap: wrap;
  }

  .travel-types label {
    font-size: 14px;
    background: white;
    padding: 5px 10px;
    border: 1px solid #ddd;
    border-radius: 8px;
    cursor: pointer;
  }

  /* Form Grid */
  .form-grid {
    display: flex;
    flex-direction: column;
    gap: 20px;
    margin-top: 10px;
  }

  .form-group {
    width: 100%;
  }

  /* Input fields */
  .input-with-icon input,
  .input-with-icon select {
    width: 100%;
    padding: 10px 10px 10px 40px;
    border: 1px solid #ddd;
    border-radius: 8px;
    font-size: 14px;
  }

  .input-with-icon i {
    font-size: 16px;
    left: 12px;
    top: 50%;
    transform: translateY(-50%);
  }

  /* Search Button */
  .form-group.full-width {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .search-btn {
    width: 100%;
    padding: 12px;
    background-color: #007bff;
    color: white;
    font-size: 16px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
  }

  /* Order of elements */
  .tabs {
    order: 1;
  }

  .travel-types {
    order: 2;
  }

  .form-group:nth-child(1) {
    order: 3; /* From */
  }

  .form-group:nth-child(2) {
    order: 4; /* To */
  }

  .form-group:nth-child(3) {
    order: 5; /* Date */
  }

  .form-group:nth-child(4) {
    order: 6; /* Person */
  }

  .form-group.full-width {
    order: 7; /* Search Button */
  }
}





/* Travel Buttons */
.tabs button .fa-plane,
.tabs button .fa-globe {
  margin-right: 8px;
  color: #040404 !important; /* Make the icons darker with !important */
}

/* Flight Button */
.tabs .flight-btn .fa-plane {
  color: #000 !important; /* Darker color for flight button icon */
}

/* Tour Button */
.tabs .tour-btn .fa-globe {
  color: #000 !important; /* Darker color for tour button icon */
}

/* Travel Buttons (hover effect for icons) */
.tabs button:hover .fa-plane,
.tabs button:hover .fa-globe {
  color: #0056b3; /* Change to blue on hover for better contrast */
}

.input-with-icon .fa-user {
  left: 12px;
}
</style>


