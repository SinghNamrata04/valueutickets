<script setup>
import Header from '../components/Header.vue'
import FlightSearch from '../components/FlightSearchResults.vue'
import Footer from '../components/Footer.vue'
</script>

<template>
  <main>
    <Header />
    <FlightSearch />  <!-- You can add comments like this within the template -->
    <Footer/>
  </main>
</template>

<style scoped>

main{  background: #F9F2F2;  }

</style>
