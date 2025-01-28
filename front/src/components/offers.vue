<template>
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
</template>

<script>
export default {
  name: "Offers",
  data() {
    return {
      offers: [
        {
          description: "*Get 12% off on your first domestic flight booking with ValueU Tickets! Book now and save big on your travel.",
          image: "/images/12percent.png", // Replace with your image path
        },
        {
          description: "*Your first flight just got more affordable! Book now and save up to 12% on flights across the USA.",
          image: "/images/mmt12.jpg",
        },
        {
          description: "*Enjoy $50 cashback when you book your first flight with ValueU Tickets! Travel smart and save more.",
          image: "/images/paytm.jpg",
        },
        {
          description: "*Book now and get $100 off on your flight! Exclusive deal only on ValueU Tickets—don't miss out.",
          image: "/images/mmt.jpg",
        },
        {
          description: "Exclusive Deals Await! Experience the ultimate sea voyage with luxury cruise deals starting at just $299! Book today for an unforgettable journey.",
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
  color: #000;
  font-weight: bold;
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
