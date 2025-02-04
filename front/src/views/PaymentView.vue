<script setup>
import Header from '../components/Header.vue'
import Footer from '../components/Footer.vue'
import PremiumSupport from '../components/PremiumSupp.vue'
import BookingNumSMS from '../components/BookingNumSMS.vue'
import Review_Itinerary from '../components/Review_Itinerary.vue'
</script>

<template>
  <main>
    <Header />
    <Review_Itinerary/>
    <PremiumSupport/>
    <BookingNumSMS/>
    <Footer/>
</main>
</template>

<style scoped>

main{  background: #FFFFFF;  }

</style>
