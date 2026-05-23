<script setup>
import Navbar from './components/Navbar.vue'
import Landing from './components/Landing.vue'
import Project from './components/Project.vue'
import Tools from './components/Tools.vue'
import Contact from './components/Contact.vue'
import Footer from './components/Footer.vue'
</script>

<template>
  <Navbar />
  <div class="main-container">
    <Landing />
    <Project />
    <Tools />
    <Contact />
  </div>
  <Footer />
</template>
