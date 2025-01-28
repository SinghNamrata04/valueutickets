<script setup>
//import TheWelcome from '../components/TeamGreeting.vue'
import Header from '../components/Header.vue'
import FlightSearch from '../components/FlightSearch.vue'
import offers from '../components/offers.vue'
import DealsReview from '../components/DealsReview.vue'
import FAQ from '../components/Faq.vue'
import Footer from '../components/Footer.vue'
import PopularDestination from'../components/popular_destination.vue'
</script>

<template>
  <main>
    <!-- <TheWelcome /> -->
    <Header />
    <FlightSearch />  <!-- You can add comments like this within the template -->
    <offers/>
    <PopularDestination />
    <DealsReview />
    <FAQ />
    <Footer/>
  </main>
</template>
