<script setup>
import { ref, watch, onMounted, onUnmounted } from 'vue'
import axios from 'axios'

// Define props for v-model binding and placeholder text
const props = defineProps({
  modelValue: {
    type: String,
    default: ''
  },
  placeholder: {
    type: String,
    default: 'Enter airport'
  }
})
const emit = defineEmits(['update:modelValue'])

// Local reactive state
const query = ref(props.modelValue)
const suggestions = ref([])
// This will hold all fetched airport data
const allAirports = ref([])

// Reference to the component's container element
const autocompleteContainer = ref(null)

// Function to fetch all airports from the API on mount
const fetchAllAirports = async () => {
  try {
    const response = await axios.get('https://gcp.agratasinfotech.com/api/v1/airports/')
    // Assuming response.data is an array of airport objects
    allAirports.value = response.data
  } catch (error) {
    console.error('Error fetching airports:', error)
    allAirports.value = []
  }
}

// Helper function to determine match priority
// Lower number means higher priority (1 is best)
const getPriority = (airport, search) => {
  const s = search.toLowerCase()
  if (airport.city?.toLowerCase().includes(s)) return 1
  if (airport.airport_name?.toLowerCase().includes(s)) return 2
  if (airport.iata?.toLowerCase().includes(s)) return 3
  if (airport.icao?.toLowerCase().includes(s)) return 4
  return 5
}

// Function to perform local filtering and sorting of airports
const filterAirports = (searchTerm) => {
  const term = searchTerm.toLowerCase()
  const filtered = allAirports.value.filter(airport => {
    return (
      airport.city?.toLowerCase().includes(term) ||
      airport.airport_name?.toLowerCase().includes(term) ||
      airport.iata?.toLowerCase().includes(term) ||
      airport.icao?.toLowerCase().includes(term)
    )
  })

  filtered.sort((a, b) => {
    const pa = getPriority(a, term)
    const pb = getPriority(b, term)
    // If same priority, sort alphabetically by city name (if available)
    if (pa === pb && a.city && b.city) {
      return a.city.localeCompare(b.city)
    }
    return pa - pb
  })
  suggestions.value = filtered
}

// Watch for changes in the query
watch(query, (newValue) => {
  emit('update:modelValue', newValue)
  // Only search if query is at least 3 characters long
  if (newValue && newValue.length >= 3) {
    filterAirports(newValue)
  } else {
    suggestions.value = []
  }
})

// When the component mounts, fetch the airports data
onMounted(() => {
  fetchAllAirports()
  // Add event listener to detect clicks outside the component
  document.addEventListener('click', handleClickOutside)
})

// Remove the event listener when the component unmounts
onUnmounted(() => {
  document.removeEventListener('click', handleClickOutside)
})

// Event handler to detect clicks outside the component
const handleClickOutside = (event) => {
  // Check if the click target is outside the component's container
  if (autocompleteContainer.value && !autocompleteContainer.value.contains(event.target)) {
    suggestions.value = []
  }
}

// When a suggestion is selected, update the input field and clear suggestions
const selectSuggestion = (suggestion) => {
  // Display both airport name and IATA (if available)
  query.value =
    suggestion.city +
    (suggestion.iata ? ` (${suggestion.iata})` : '')
  suggestions.value = []
  emit('update:modelValue', query.value)
}
</script>

<template>
  <!-- Bind the container ref here -->
  <div class="airport-autocomplete" ref="autocompleteContainer">
    <input 
      type="text" 
      :placeholder="placeholder" 
      v-model="query" 
      autocomplete="off"
    />
    <ul v-if="suggestions.length">
      <li 
        v-for="airport in suggestions" 
        :key="airport.id" 
        @click="selectSuggestion(airport)"
      >
        <span v-if="airport.iata">{{ airport.iata }} - </span>
        {{ airport.city }} - {{ airport.airport_name }}
      </li>
    </ul>
  </div>
</template>

<style scoped>
.airport-autocomplete {
  position: relative;
  width: 100%;
}
.airport-autocomplete input {
  width: 100%;
  height: 100%;
  padding: 0px;
  margin: 0px;
  padding-left: 3px;
  font-size: 19px;
  box-sizing: border-box;
  border: 0px solid #00000000;
}
.airport-autocomplete ul {
  position: absolute;
  top: 120%;
  left: -100px;
  right: 0;
  width: 200%;
  background: #fff;
  border: 1px solid #000000;
  border-radius: 10px;
  max-height: 250px;
  overflow-y: auto;
  z-index: 1000;
  margin: 0;
  padding: 0;
  list-style: none;
}
.airport-autocomplete li {
  padding: 5px;
  border-block: 1px solid #000000;
  cursor: pointer;
}
.airport-autocomplete li:hover {
  background-color: #f0f0f0;
}
</style>
