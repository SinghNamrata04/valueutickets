<template>
  <div class="offers-container">
    <h2 class="offers-title">Exclusive Offers</h2>
    <div class="offers-grid">
      <div
        v-for="(offer, index) in visibleOffers"
        :key="index"
        class="offer-card"
        @click="handleCardClick(offer)"
      >
        <img :src="offer.image" :alt="offer.title" class="offer-image" />
        <div class="offer-content">
          <h3 class="offer-title">{{ offer.title }}</h3>
          <p class="offer-description">{{ offer.description }}</p>
        </div>
      </div>
    </div>
    <div class="expand-btn-container">
      <button class="expand-btn" @click="toggleOffers">
        {{ showAll ? 'Show Less' : 'Show More' }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Offers",
  data() {
    return {
      offers: [
        {
          title: "Beach Paradise",
          description: "Enjoy 20% off on beach resorts.",
          image: "/images/beach.jpg", // Replace with your image path
        },
        {
          title: "Mountain Adventure",
          description: "Explore the mountains with exclusive deals.",
          image: "/images/dark.jpg",
        },
        {
          title: "City Getaway",
          description: "Discover city life with amazing discounts.",
          image: "/images/city.jpg",
        },
        {
          title: "Tropical Escape",
          description: "Book now and save on tropical vacations.",
          image: "/images/tropical.jpg",
        },
        {
          title: "Luxury Cruise",
          description: "Exclusive deals on luxury cruises.",
          image: "/images/cruise.jpg",
        },
      ],
      visibleRows: 1, // Number of rows initially visible
      offersPerRow: 3, // Number of offers per row
      showAll: false, // To toggle between show more and show less
    };
  },
  computed: {
    visibleOffers() {
      return this.showAll
        ? this.offers
        : this.offers.slice(0, this.visibleRows * this.offersPerRow);
    },
  },
  methods: {
    toggleOffers() {
      this.showAll = !this.showAll;
    },
    handleCardClick(offer) {
      alert(`You clicked on: ${offer.title}`); // Replace with desired navigation logic
    },
  },
};
</script>

<style scoped>
.offers-container {
  margin: 70px auto;
  max-width: 1200px;
  padding: 20px;
  border-radius: 12px;
  background:rgba(249, 249, 249, 0);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0);
  z-index: 1;
  position: relative;
}

.offers-title {
  text-align: center;
  font-size: 24px;
  color: #333;
  margin-bottom: 20px;
}

.offers-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
}

.offer-card {
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  cursor: pointer;
}

.offer-image {
  width: 100%;
  height: 150px;
  object-fit: cover;
}

.offer-content {
  padding: 15px;
}

.offer-title {
  font-size: 18px;
  color: #007bff;
  margin: 0 0 10px;
}

.offer-description {
  font-size: 14px;
  color: #666;
  margin: 0;
}

.expand-btn-container {
  text-align: center;
  margin-top: 20px;
}

.expand-btn {
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
}

.expand-btn:hover {
  background-color: #0056b3;
}
</style>