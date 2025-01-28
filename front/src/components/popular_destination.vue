<template>
  <div class="container">
    <h2 class="title">Popular Destination</h2>

    <!-- Anchor Link for View All/View Less -->
    <a @click="toggleView" class="toggle-link">
      {{ showAll ? "View Less" : "View All" }}
    </a>

    <div class="grid">
      <div 
        v-for="(destination, index) in visibleDestinations" 
        :key="index" 
        class="card"
        :class="{ 'large-card': destination.isLarge }"
      >
        <img :src="destination.image" :alt="destination.name" />
        <div class="overlay">
          <h3>{{ destination.name }}</h3>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// Importing images
import LondonImage from '@/assets/footer/London_D.jpg';
import ParisImage from '@/assets/footer/Paris_D.jpg';
import JapanImage from '@/assets/footer/japan_D.jpg';
import CancunImage from '@/assets/footer/Cancun_D.jpg';
import SanJuanImage from '@/assets/footer/maxico_D.jpg';

export default {
  data() {
    return {
      showAll: false,
      destinations: [
        { name: "London, United Kingdom", image: LondonImage, isLarge: false },
        { name: "Paris, France", image: ParisImage, isLarge: true },
        { name: "Japan, Tokyo", image: JapanImage, isLarge: false },
        { name: "Cancun, Mexico", image: CancunImage, isLarge: false },
        { name: "San-Jaun, Puerto Rico", image: SanJuanImage, isLarge: false },
        { name: "London, United Kingdom", image: LondonImage, isLarge: false },
        { name: "Paris, France", image: ParisImage, isLarge: true },
        { name: "Japan, Tokyo", image: JapanImage, isLarge: false },
        { name: "Cancun, Mexico", image: CancunImage, isLarge: false },
        { name: "San-Jaun, Puerto Rico", image: SanJuanImage, isLarge: false },
        { name: "London, United Kingdom", image: LondonImage, isLarge: false },
        { name: "Paris, France", image: ParisImage, isLarge: true },
        { name: "Japan, Tokyo", image: JapanImage, isLarge: false },
        { name: "Cancun, Mexico", image: CancunImage, isLarge: false },
        { name: "San-Jaun, Puerto Rico", image: SanJuanImage, isLarge: false }
      ]
    };
  },
  computed: {
    visibleDestinations() {
      return this.showAll ? this.destinations : this.destinations.slice(0, 5);
    }
  },
  methods: {
    toggleView() {
      this.showAll = !this.showAll;
    }
  }
};
</script>

<style scoped>
.container {
  text-align: center;
  padding: 20px;
  position: relative; 
  background-color: #F9F2F2; 
}

.title {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 10px;
  position: absolute; 
  top: 20px; 
  left: 20px; 
}

/* ✅ Anchor Link Styling */
.toggle-link {
  position: absolute; 
  top: 40px; 
  right: 20px; 
  color: #007bff;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  transition: color 0.3s ease;
}

.toggle-link:hover {
  color: #0056b3;
}

/* ✅ Grid Layout */
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: 250px auto;
  gap: 15px;
  align-items: center;
  margin-top: 50px; 
}

.card {
  position: relative;
  overflow: hidden;
  border-radius: 10px;
}

.card img {
  width: 100%;
  height: auto;
  max-height: 250px;
  object-fit: cover;
  border-radius: 10px;
}

.large-card {
  grid-column: span 1;
  grid-row: span 2;
}

.large-card img {
  height: auto;
  max-height: 515px;
  object-fit: cover;
}

.overlay {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba(0, 0, 0, 0.6);
  color: white;
  padding: 15px;
  text-align: center;
  font-size: 18px;
  font-weight: bold;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.card:hover .overlay {
  opacity: 1;
}

/* ✅ Responsive Design */
@media (max-width: 768px) {
  .grid {
    grid-template-columns: repeat(2, 1fr);
    grid-template-rows: auto;
  }

  .large-card {
    grid-column: span 2;
    grid-row: auto;
  }
}

@media (max-width: 480px) {
  .grid {
    grid-template-rows: auto;
    grid-template-columns: repeat(2, 1fr);
  }

  .card img {
    max-height: 200px;
  }

  .large-card img {
    max-height: 200px;
  }

  .title {
    font-size: 20px;
    
  }

  .toggle-link {
    font-size: 15px;
  }
}
</style>
