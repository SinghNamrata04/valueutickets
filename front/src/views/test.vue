<template>
  <div class="airport-search">
    <h2>Airport Search</h2>
    <!-- Search Input -->
    <div class="search-container" @click.outside="hideDropdown">
      <input
        type="text"
        v-model="searchQuery"
        @focus="dropdownVisible = true"
        @input="handleInput"
        placeholder="Type to search airports..."
      />

      <!-- Dropdown with filtered results -->
      <ul v-if="dropdownVisible && filteredAirports.length" class="dropdown">
        <li
          v-for="airport in filteredAirports"
          :key="airport.id"
          @click="selectAirport(airport)"
        >
          <strong>{{ airport.airport_name }}</strong> - {{ airport.city }} <br />
          <small>
            IATA: {{ airport.iata }} | ICAO: {{ airport.icao }} | FAA: {{ airport.faa }}
          </small>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "AirportSearch",
  data() {
    return {
      searchQuery: "",
      airports: [],
      dropdownVisible: false
    };
  },
  computed: {
    // Computed property to filter airports based on searchQuery
    filteredAirports() {
      if (!this.searchQuery) {
        return [];
      }
      const query = this.searchQuery.toLowerCase();
      return this.airports.filter((airport) => {
        return (
          (airport.city && airport.city.toLowerCase().includes(query)) ||
          (airport.airport_name && airport.airport_name.toLowerCase().includes(query)) ||
          (airport.faa && airport.faa.toLowerCase().includes(query)) ||
          (airport.iata && airport.iata.toLowerCase().includes(query)) ||
          (airport.icao && airport.icao.toLowerCase().includes(query))
        );
      });
    }
  },
  methods: {
    // Handle input event and show dropdown
    handleInput() {
      this.dropdownVisible = true;
    },
    // When an airport is selected, update the search query and hide the dropdown
    selectAirport(airport) {
      this.searchQuery = airport.airport_name;
      this.dropdownVisible = false;
    },
    // Hide the dropdown when clicking outside
    hideDropdown() {
      this.dropdownVisible = false;
    }
  },
  mounted() {
    axios
      .get("https://gcp.agratasinfotech.com/api/v1/airports/")
      .then((response) => {
        this.airports = response.data;
      })
      .catch((error) => {
        console.error("Error fetching airports:", error);
      });
  },
  // A simple directive to detect clicks outside of an element
  directives: {
    outside: {
      beforeMount(el, binding, vnode) {
        el.clickOutsideEvent = function (event) {
          // Check that click was outside the el and its children
          if (!(el === event.target || el.contains(event.target))) {
            binding.value(event);
          }
        };
        document.body.addEventListener("click", el.clickOutsideEvent);
      },
      unmounted(el) {
        document.body.removeEventListener("click", el.clickOutsideEvent);
      }
    }
  }
};
</script>

<style scoped>
.airport-search {
  max-width: 600px;
  margin: 2rem auto;
  font-family: Arial, sans-serif;
}

.search-container {
  position: relative;
}

input[type="text"] {
  width: 100%;
  padding: 8px;
  font-size: 1rem;
  box-sizing: border-box;
}

.dropdown {
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  max-height: 300px;
  overflow-y: auto;
  background: #fff;
  border: 1px solid #ccc;
  z-index: 1000;
  list-style: none;
  margin: 0;
  padding: 0;
}

.dropdown li {
  padding: 8px;
  cursor: pointer;
}

.dropdown li:hover {
  background: #f0f0f0;
}
</style>
