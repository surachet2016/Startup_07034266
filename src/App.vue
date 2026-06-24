<script setup>
import NavBar from './components/NavBar.vue'
import HeroSection from './components/HeroSection.vue'
import WeekSection from './components/WeekSection.vue'
import FooterBar from './components/FooterBar.vue'
import { weeks } from './data/weeks.js'
</script>

<template>
  <NavBar />
  <HeroSection />
  <main>
    <WeekSection v-for="w in weeks" :key="w.week" :week="w" />
  </main>
  <FooterBar />
</template>
