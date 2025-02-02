<script setup>
import passclass from './passclass.vue'
import AirportAuto from './AirportAuto.vue'
</script>

<template>
  <div class="container">
    <div class="tabs">
      <button @click="activeTab = 'flight'" :class="{ active: activeTab === 'flight' }">
        <i class="fas fa-plane"></i> Flight
      </button>
      <button @click="activeTab = 'tour'" :class="{ active: activeTab === 'tour' }">
        <i class="fas fa-globe"></i> Tour
      </button>
    </div>
    
    <div v-if="activeTab === 'flight'">
      <div class="trip-type">
        <label v-for="type in ['oneWay', 'roundTrip', 'multiCity']" :key="type">
          <input type="radio" v-model="tripType" :value="type" /> {{ formatTripType(type) }}
        </label>
      </div>
      
      <!-- One Way Trip -->
      <div v-if="tripType === 'oneWay'" class="input-group">
        <div class="input-box">
          <label>From</label>
          <span>
            <i class="fas fa-plane-departure"></i>
            <!-- Replace plain input with AirportAuto -->
            <AirportAuto 
              placeholder="From" 
              v-model="oneWay.from" 
            />
          </span>
          <button class="swap-btn" @click="swap(oneWay)">🔄</button>
        </div>
        <div class="input-box">
          <label>To</label>
          <span>
            <i class="fas fa-plane-arrival"></i>
            <AirportAuto 
              placeholder="To" 
              v-model="oneWay.to" 
            />
          </span>
        </div>
        <div class="input-box">
          <label>Departure Date</label>
          <span>
            <i class="fas fa-calendar-alt"></i>
            <input type="date" v-model="oneWay.date" :min="today" />
          </span>
        </div>
        <div class="input-box">
          <label>Travellers, Class</label>
          <passclass />
        </div>
      </div>
      
      <!-- Round Trip -->
      <div v-if="tripType === 'roundTrip'" class="input-group">
        <div class="input-box">
          <label>From</label>
          <span>
            <i class="fas fa-plane-departure"></i>
            <AirportAuto 
              placeholder="From" 
              v-model="roundTrip.from" 
            />
          </span>
          <button class="swap-btn" @click="swap(roundTrip)">🔄</button>
        </div>
        <div class="input-box">
          <label>To</label>
          <span>
            <i class="fas fa-plane-arrival"></i>
            <AirportAuto 
              placeholder="To" 
              v-model="roundTrip.to" 
            />
          </span>
        </div>
        <div class="input-box">
          <label>Departure Date</label>
          <span>
            <i class="fas fa-calendar-alt"></i>
            <input type="date" v-model="roundTrip.date" :min="today" />
          </span>
        </div>
        <div class="input-box">
          <label>Return Date</label>
          <span>
            <i class="fas fa-calendar-alt"></i>
            <input type="date" v-model="roundTrip.returnDate" :min="roundTrip.date" />
          </span>
        </div>
        <div class="input-box">
          <label>Travellers, Class</label>
          <passclass />
        </div>
      </div>
      
      <!-- Multi City -->
      <div v-if="tripType === 'multiCity'">
        <div v-for="(segment, index) in multiCity" :key="index" class="multi-city-segment input-group">
          <div class="input-box">
            <label>From</label>
            <span>
              <i class="fas fa-plane-departure"></i>
              <AirportAuto 
                placeholder="From" 
                v-model="segment.from"
                @input="autoFillFrom(index)" 
              />
            </span>
            <div class="swap-btn" @click="swap(segment)">🔄</div>
          </div>
          <div class="input-box">
            <label>To</label>
            <span>
              <i class="fas fa-plane-arrival"></i>
              <AirportAuto 
                placeholder="To" 
                v-model="segment.to" 
              />
            </span>
          </div>
          <div class="input-box">
            <label>Departure Date</label>
            <span>
              <i class="fas fa-calendar-alt"></i>
              <input type="date" v-model="segment.date" :min="today" />
            </span>
          </div>
          <div class="input-box" v-if="index === 0">
            <label>Travellers, Class</label>
            <passclass />
          </div>
          <button v-if="index === 1 || index >= 2" @click="addSegment"><i class="fa fa-plus"></i></button>
          <button v-if="index >= 2" @click="removeSegment(index)"><i class="fa fa-minus"></i></button>
        </div>
      </div>
      
      <router-link to="/results"><button class="search-button">Search Flight</button></router-link>
    </div>
    
    <div v-else>
      <p>Tour section (same content for now)</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      activeTab: 'flight',
      tripType: 'oneWay',
      today: new Date().toISOString().split('T')[0],
      oneWay: { from: '', to: '', date: '', travelClass: '' },
      roundTrip: { from: '', to: '', date: '', returnDate: '', travelClass: '' },
      multiCity: [
        { from: '', to: '', date: '', travelClass: '' },
        { from: '', to: '', date: '' }
      ]
    };
  },
  methods: {
    formatTripType(type) {
      return type === 'oneWay' ? 'One Way' : type === 'roundTrip' ? 'Round Trip' : 'Multi-City';
    },
    addSegment() {
      if (this.multiCity.length < 4) {
        this.multiCity.push({ from: '', to: '', date: '' });
      }
    },
    removeSegment(index) {
      this.multiCity.splice(index, 1);
    },
    autoFillFrom(index) {
      if (index > 0 && !this.multiCity[index].from) {
        this.multiCity[index].from = this.multiCity[index - 1].to;
      }
    },
    swap(segment) {
      [segment.from, segment.to] = [segment.to, segment.from];
    }
  }
};
</script>

<style scoped>
/* (Your existing styles remain unchanged) */
.container {
  padding: 5px 15px;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #FFFFFF;
  border-radius: 15px;
}
.tabs {
  display: flex;
  gap: 20px;
  margin-bottom: 10px;
  justify-content: center;
}
.tabs button {
  padding: 10px;
  margin: 5px;
  cursor: pointer;
  border-radius: 10px;
  display: flex;
  align-items: center;
  gap: 5px;
  background-color: transparent;
  border: none;
}
.tabs i {
  font-size: 18px;
}
.active {
  font-weight: bold;
  text-decoration: underline;
  color: #007bff;
}
.flight-section {
  width: 100%;
  display: flex;
  justify-content: center;
}
.trip-type {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
  gap: 10px;
}
.trip-type label {
  cursor: pointer;
}
.input-group {
  display: flex;
  gap: 5px;
  justify-self: center;
  margin-bottom: 10px;
  justify-content: center;
}
.input-box {
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
}
.input-box label {
  margin-bottom: 5px;
  font-weight: bold;
  text-align: left;
  width: 100%;
  padding-left: 5px;
}
.input-box span {
  display: flex;
  align-items: center;
  width: 200px;
  height: 50px;
  padding: 10px;
  border-radius: 10px;
  border: 1px solid #000;
  background: #fff;
  font-size: 15px;
  justify-content: center;
}
.input-box input[type="text"],
.input-box input[type="date"] {
  border: none;
  outline: none;
  font-size: 18px;
  width: 100%;
}
.input-box i {
  margin-right: 5px;
}
.swap-btn {
  background: none;
  border: none;
  font-size: 22px;
  cursor: pointer;
  position: absolute;
  z-index: 100;
  right: -23px;
  top: 36px;
}
.search-button {
  display: block;
  margin: 20px auto;
  padding: 10px 20px;
  cursor: pointer;
  border-radius: 10px;
  background-color: #007bff;
  color: white;
  border: none;
}
.multi-city-segment {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
  width: 100%;
  justify-content: flex-start;
}
.multi-city-segment .input-group {
  justify-content: flex-start;
}
.multi-city-segment button {
  background-color: #007bff;
  color: white;
  border: none;
  line-height: 30px;
  font-size: 30px;
  margin-top: 35px;
  width: 40px;
  height: 40px;
  border-radius: 50%;
}
.trip-type label {
  display: flex;
  align-items: center;
  justify-content: center;
}
input[type="radio"] {
  margin-right: 5px;
}
</style>
