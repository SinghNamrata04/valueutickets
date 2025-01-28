<template>
  <div class="page-container">
    <!-- Best Deals Section -->
    <div class="flight-deals">
      <h2 class="section-title">Best Deals on Flight Tickets</h2>
      <ul class="deals-list">
        <li v-for="(deal, index) in visibleDeals" :key="index" class="deal-item">
          <!-- Flight Details -->
          <div class="deal-details">
            <h3 class="route">{{ deal.route }}</h3>
            <p class="details">{{ deal.date }} | {{ deal.airline }}</p>
          </div>
          <!-- Pricing -->
          <div class="deal-price">
            <span class="Span_deal">One way as low as</span>
            <strong class="price">${{ deal.price }}</strong>
          </div>
          <!-- Book Now Button -->
          <button class="book-now">Book Now</button>
        </li>
      </ul>
      <!-- Expand and Collapse Button -->
      <button v-if="!expanded" class="view-all" @click="expandDeals">View All</button>
      <button v-if="expanded" class="view-all" @click="collapseDeals">View Less</button>
    </div>

    <!-- Reviews Section -->
    <h2 class="section-title">Reviews & Ratings</h2>
    <div class="reviews">
      <div v-for="(review, index) in reviews" :key="index" class="review-card">
        <div class="review-header">
          <div class="avatar">{{ review.initials }}</div>
          <div>
            <h3 class="review-name">{{ review.name }}</h3>
            <div class="review-rating">
  <span
    v-for="n in 5"
    :key="n"
    class="star"
    :class="{ filled: n <= review.rating }"
  >
    ★
  </span>
</div>

            <p class="review-date">{{ review.date }}</p>
          </div>
        </div>
        <p class="review-text">{{ review.text }}</p>
        
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      allDeals: [
        { route: "New York - Japan", date: "Thursday 30 Jan 2025", airline: "Indigo", price: "57.55" },
        { route: "Maxico - Japan", date: "Thursday 30 Jan 2025", airline: "Indigo", price: "57.55" },
        { route: "London - Japan", date: "Thursday 30 Jan 2025", airline: "Indigo", price: "87.55" },
        { route: "New York - Japan", date: "Thursday 30 Jan 2025", airline: "Indigo", price: "59.55" },
        { route: "New York - Japan", date: "Thursday 30 Jan 2025", airline: "Indigo", price: "57.55" },
        { route: "New York - Japan", date: "Thursday 30 Jan 2025", airline: "Indigo", price: "59.55" },
        { route: "New York - Japan", date: "Thursday 30 Jan 2025", airline: "Indigo", price: "57.55" },
        { route: "Los Angeles - Japan", date: "Thursday 30 Jan 2025", airline: "Indigo", price: "67.55" },
        { route: "Chicago - Japan", date: "Thursday 30 Jan 2025", airline: "Indigo", price: "77.55" },
         { route: "New York - Japan", date: "Thursday 30 Jan 2025", airline: "Indigo", price: "59.55" },
        { route: "New York - Japan", date: "Thursday 30 Jan 2025", airline: "Indigo", price: "57.55" },
        { route: "New York - Japan", date: "Thursday 30 Jan 2025", airline: "Indigo", price: "59.55" },
        { route: "New York - Japan", date: "Thursday 30 Jan 2025", airline: "Indigo", price: "57.55" },
        { route: "Los Angeles - Japan", date: "Thursday 30 Jan 2025", airline: "Indigo", price: "67.55" },
        { route: "Chicago - Japan", date: "Thursday 30 Jan 2025", airline: "Indigo", price: "77.55" },
      ],
      visibleDeals: [],
      expanded: false,
       reviews: [
        { name: "Jc Castillo", date: "April 14, 2025", text: "The booking process was seamless, and the flight was on time! Thank you for the excellent service.", initials: "JC", rating: 5 },
        { name: "Jane Doe", date: "April 15, 2025", text: "The crew was very polite, but there was a slight delay. Overall, it was a good experience." , initials: "JD", rating: 4 },
        { name: "John Smith", date: "April 16, 2025", text: "It was a very smooth journey. I highly recommend your service!", initials: "JS", rating: 3 },
      ],
       
    };
  },
  mounted() {
    this.visibleDeals = this.allDeals.slice(0, 5); // Show first 5 deals by default
  },
  methods: {
    expandDeals() {
      this.visibleDeals = this.allDeals;
      this.expanded = true;
    },
    collapseDeals() {
      this.visibleDeals = this.allDeals.slice(0, 5);
      this.expanded = false;
    },
  },
};
</script>

<style scoped>
/* Global Styles */
.page-container {
  background-color: #f9f2f2;
  padding-bottom: 20px;
  padding-top:20px;
  font-family: Arial, sans-serif;
}

/* Section Title */
.section-title {
  font-size: 24px;
  margin-bottom: 20px;
  text-align: center;
  color: #333;
  text-decoration:underline;
}

/* Best Deals Section */
.flight-deals {
  background-color: #fff; 
  padding: 20px;
  width: 95%; 
  margin: 0 auto 40px auto;

}

.deals-list {
  list-style: none;
  padding: 0;
}

.deal-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 0;
}

.route {
  font-size: 24px;
  margin-left: 50px;
}

.details {
  font-size: 18px;
  color: #555;
  margin: 5px 0 0;
  margin-left: 50px;
}

.deal-price {
  text-align: center;
  margin-top: 3rem;
  
}
.price{
  margin-top: -2.2rem;
}

.deal-price span {
  font-size: 16px;
  color: #777;
  margin-bottom: 20px;
}

.deal-price strong {
  font-size: 22px; 
  color: #ff5722;
  display: block;
  margin-left: 25rem;
  text-align: center;
  margin-bottom: 20px;
}

.book-now {
  background: linear-gradient(90deg, #3E41EC, #09C398);
  border: none;
  padding: 10px 20px;
  color: #fff;
  border-radius: 50px;
  cursor: pointer;
  animation: pulse 1.5s infinite; /* Infinite animation */
  transition: background 0.3s ease;
  margin-right: 70px;
}

/* Pulse Animation */
@keyframes pulse {
  0% {
    transform: scale(1); /* Original size */
  }
  50% {
    transform: scale(1.1); /* Slightly larger */
  }
  100% {
    transform: scale(1); /* Back to original size */
  }
}

/* Hover Effect  */
.book-now:hover {
  background: linear-gradient(90deg, #00ced1, #1e90ff);
}

/* After Hover Effect  */
.book-now:hover {
  animation: bounce 0.6s ease;
}

@keyframes bounce {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-5px); /* Move up slightly */
  }
}


.view-all {
  margin: 20px auto;
  display: block;
  padding: 10px 20px;
  color: #1e90ff;
  border: 1px solid #1e90ff;
  background: #fff;
  border-radius: 5px;
  cursor: pointer;
}

/* Reviews Section */
.reviews {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  max-width: 900px;
  margin: auto;
}

.review-card {
  border: 1px solid #ddd;
  padding: 15px;
  border-radius: 5px;
  background: #fff;
}

/* Star Rating Styles */
.review-rating {
  margin-top: 20px;
  font-size: 18px;
}

.star {
  color: #ddd; 
}

.star.filled {
  color: gold !important;
}

.review-header {
  display: flex;
  align-items: center;
}

.avatar {
  width: 40px;
  height: 40px;
  background: #1e90ff;
  color: #fff;
  font-weight: bold;
  font-size: 16px;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 50%;
  margin-right: 10px;
  margin-bottom: 3rem ;
}

.review-name {
  font-size: 16px;
  margin-bottom: -1rem ;

}

.review-date {
  font-size: 12px;
  color: #777;
  margin-bottom: 1rem;
}

.review-text {
  margin-top: 10px;
  color: #555;
  font-size: 14px;
}
/*-----------------Responsive Design of Best Deals Section----------------*/
@media(max-width:992px){
/* Best Deals Section */
.flight-deals {
  padding: 10px;
  width: 90%; /* Center content */
}
.route {
  margin-left: 20px;
}
.details {
  margin-left: 20px;
}
.deal-price strong {
  margin-left: 15rem;
}
.book-now {
 
  margin-right: 20px
}
.deal-price span {
 
  margin-right:4rem;
  }
}
@media(max-width:768px){
.flight-deals {
  padding: 10px;
  width: 95%; 
}
.route {
  margin-left: 10px;
}
.details {
  margin-left: 10px;
}
.deal-price strong {
  font-size: 20px;
  margin-left: 10rem;
   margin-bottom: 38px;
}
.book-now {
  margin-right: 20px
}
}
@media(max-width:675px){
.flight-deals {
  padding: 10px;

}
.route {
  margin-left: 10px;
  font-size: 16px;
  margin-bottom: 0.5rem;
  margin-top: -1rem;
}
.details {
  margin-left: 10px;
  font-size: 12px;
  margin-bottom: -2rem;
}
.deal-price strong {
  font-size: 16px;
  margin-left: 9rem;
}
.book-now {
  font-size: 8px;
  margin-right: 10px;
  margin-left: 10px;
}
}
@media(max-width:425px){
 .flight-deals {
  padding: 10px;
  width: 100%; 
} 
/* Section Title */
.section-title {
  font-size: 20px;
  margin-right: -10rem;
}
.flight-deals {
  padding: 10px;
  width: 100%; 
  }
  .route {
  font-size: 14px;
  margin-left: 5px;
}
.details {
  font-size: 12px;
  margin-left: 5px;
}
.deal-price strong {
  font-size: 18px;
  margin-left: 10rem;
  margin-bottom: 10px;
}
.book-now {
  
  padding: 5px 20px;
  font-size:12px;
  margin-left: 15px;
}
.view-all {
  margin: auto;
}
}


/*-----------------Responsive Design for Review Section----------------*/
@media(max-width:992px){
.reviews {
  gap: 40px;
  width: 400px;
}

}
@media(max-width:425px){
.reviews {
  width: 300px; 
}
.page-container {
  padding-bottom: 20px;
  width: 160vw;
  
}
.section-title{
  font-size:18px;
  text-align:center;
  margin-right:3rem;
}
}
</style>