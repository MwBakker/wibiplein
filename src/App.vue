<script setup>
import NavBar from '@/components/NavBar.vue';
import ContactForm from '@/components/ContactForm.vue';
import Intro from '@/components/Intro.vue';
import Footer from './components/Footer.vue';
import Cards from './components/Cards.vue';
import Deposits from './components/Deposits.vue'
</script>

<template>
  <NavBar />
  <div id="content">
    <Intro />
    <Cards />
    <Deposits />
    <ContactForm />
  </div>
  <Footer />
</template>

<style scoped>
#content {
  max-width: 1280px;
  margin: 0 auto;
  position: relative;
  padding-top: 320px;
}
</style>