<template>
  <main class="app-container">
    <!-- ==========================================================================
         GLOBAL HEADER
         ========================================================================== -->
    <Header />

    <!-- ==========================================================================
         PAGE COMPONENTS (RENDERED IN NATURAL FLOW)
         ========================================================================== -->
    <Hero />
    <Catalog />
    <Expertise />
    <Reviews />
    
    <!-- ==========================================================================
         GLOBAL FOOTER
         ========================================================================== -->
    <Footer />
  </main>
</template>

<script setup>
import { onMounted, onUnmounted, provide, shallowRef } from 'vue';
import Lenis from 'lenis';

// ==========================================================================
// COMPONENT IMPORTS
// ==========================================================================
import Header from './components/Header.vue';
import Hero from './components/Hero.vue';
import Expertise from './components/Expertise.vue';
import Catalog from './components/Catalog.vue';
import Reviews from './components/Reviews.vue';
import Footer from './components/Footer.vue';

// ==========================================================================
// LENIS SMOOTH SCROLL INITIALIZATION
// ==========================================================================
const lenis = shallowRef(null);
provide('lenis', lenis);

let rafId;

// GLOBAL ANCHOR LINK CLICK INTERCEPTOR
const handleAnchorClick = (e) => {
  if (e.defaultPrevented) return;

  const anchor = e.target.closest('a');
  
  if (anchor && anchor.getAttribute('href')?.startsWith('#')) {
    const target = anchor.getAttribute('href');
    
    e.preventDefault(); 
    
    if (target === '#top' || target === '#') {
      lenis.value?.scrollTo(0, {
        duration: 1.5,
        easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t))
      });
    } else {
      lenis.value?.scrollTo(target, {
        offset: -100, 
        duration: 1.5, 
        easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t))
      });
    }
  }
};

onMounted(() => {
  lenis.value = new Lenis({
    duration: 1.2, 
    easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)), 
    orientation: 'vertical',
    gestureOrientation: 'vertical',
    smoothWheel: true,
    wheelMultiplier: 1,
    touchMultiplier: 2, 
  });

  function raf(time) {
    lenis.value?.raf(time);
    rafId = requestAnimationFrame(raf);
  }

  rafId = requestAnimationFrame(raf);
  document.addEventListener('click', handleAnchorClick);
});

onUnmounted(() => {
  if (lenis.value) {
    lenis.value.destroy();
  }
  if (rafId) {
    cancelAnimationFrame(rafId);
  }
  document.removeEventListener('click', handleAnchorClick);
});
</script>

<style scoped>
/* ==========================================================================
   APP CONTAINER (GLOBAL ROOT)
   ========================================================================== */
.app-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  width: 100%;
  max-width: 100vw;
  overflow-x: hidden;
  background-color: #ffffff;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* ==========================================================================
   SMOOTH SCROLL OFFSET FIX
   ========================================================================== */
:deep(section),
:deep(footer) {
  scroll-margin-top: 100px;
}
</style>