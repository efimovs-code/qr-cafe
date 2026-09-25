<template>
  <!-- Bind dynamic height in px via JS -->
  <section class="hero-section" :style="{ minHeight: heroHeight }">
    <!-- Decorative background glow (Warm cozy tint) -->
    <div class="ambient-glow"></div>

    <div class="hero-container">
      
      <!-- ==========================================================================
           CONTENT BLOCK
           ========================================================================== -->
      <div class="hero-content">
        <!-- Main Title -->
        <h1 class="hero-title animate-title">
          Затишно. Смачно.<br/>Як вдома.
        </h1>
        
        <!-- Description -->
        <p class="hero-description animate-desc">
          Великі порції, домашня кухня та атмосфера, в яку хочеться повертатися.
        </p>
        
        <!-- Action Buttons -->
        <div class="hero-actions animate-actions">
          <div class="btn-wrapper">
            <!-- Opens booking modal -->
            <button class="btn btn-primary" @click="openModal">
              Забронювати стіл
            </button>
          </div>
          
          <div class="btn-wrapper">
            <!-- Scrolls to menu -->
            <a 
              href="#menu" 
              class="btn btn-outline"
              @click="handleScrollTo($event, '#menu')"
            >
              Переглянути меню
            </a>
          </div>
        </div>
      </div>

      <!-- ==========================================================================
           PREMIUM PURE CSS VISUAL (Levitating pancakes)
           ========================================================================== -->
      <div class="hero-visual animate-visual">
        <div class="css-food-magic">
          
          <!-- Background warm glow from hot dish -->
          <div class="food-aura"></div>

          <!-- Meal Composition -->
          <div class="meal-composition">
            
            <!-- Levitating ingredients (Mint, berries) -->
            <div class="flying-ingredients">
              <div class="leaf mint-1"></div>
              <div class="leaf mint-2"></div>
              <div class="berry red-berry b1"></div>
              <div class="berry blue-berry b2"></div>
              <div class="berry red-berry b3"></div>
            </div>

            <!-- Plate -->
            <div class="plate">
              <div class="plate-inner"></div>
            </div>
            
            <!-- Pancakes (3D effect achieved through shadows instead of transform) -->
            <div class="pancake p1"></div>
            <div class="pancake p2">
              <div class="syrup-drop drop-1"></div>
            </div>
            <div class="pancake p3">
              <div class="syrup-drop drop-2"></div>
              
              <!-- Cream / Sour cream / Butter blob -->
              <div class="cream-blob"></div>
            </div>

          </div>
          
        </div>
      </div>

    </div>

    <!-- ==========================================================================
         MODAL TELEPORT
         ========================================================================== -->
    <Teleport to="body">
      <ContactModal :isOpen="isModalOpen" @close="isModalOpen = false" />
    </Teleport>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted, inject } from 'vue';
import ContactModal from './ContactModal.vue';

const isModalOpen = ref(false);
const lenis = inject('lenis');

const openModal = () => {
  isModalOpen.value = true;
};

// ==========================================================================
// SMOOTH SCROLL LOGIC
// ==========================================================================
const handleScrollTo = (e, target) => {
  e.preventDefault(); 

  const isMobile = window.innerWidth <= 992;

  if (lenis && lenis.value) {
    lenis.value.scrollTo(target, {
      offset: -80, 
      duration: isMobile ? 1.2 : 0.8,
      easing: (t) => 1 - Math.pow(1 - t, 10) 
    });
  } else {
    document.querySelector(target)?.scrollIntoView({ behavior: 'smooth' });
  }
};

// ==========================================================================
// FULLSCREEN HEIGHT LOGIC
// ==========================================================================
const heroHeight = ref('100vh'); 
let lastWidth = 0;
let resizeTimeout = null;

const calculateHeight = () => {
  if (resizeTimeout) {
    clearTimeout(resizeTimeout);
  }

  resizeTimeout = setTimeout(() => {
    const currentWidth = window.innerWidth;
    const isMobile = currentWidth <= 992;
    
    if (isMobile && currentWidth === lastWidth) {
      return; 
    }
    
    heroHeight.value = `${window.innerHeight}px`;
    lastWidth = currentWidth;
  }, 150);
};

onMounted(() => {
  heroHeight.value = `${window.innerHeight}px`;
  lastWidth = window.innerWidth;
  window.addEventListener('resize', calculateHeight);
});

onUnmounted(() => {
  if (resizeTimeout) clearTimeout(resizeTimeout);
  window.removeEventListener('resize', calculateHeight);
});
</script>

<style scoped>
/* ==========================================================================
   SCROLL FIXES
   ========================================================================== */
*, *::before, *::after {
  box-sizing: border-box;
}

/* ==========================================================================
   LAYOUT & TYPOGRAPHY
   ========================================================================== */
.hero-section {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  max-width: 100vw;
  background-color: #0F0D0C;
  overflow: hidden; 
  z-index: 10;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.ambient-glow {
  position: absolute;
  top: 50%;
  left: 50%;
  margin-left: -400px;
  margin-top: -400px;
  width: 800px;
  height: 800px;
  background: radial-gradient(circle, rgba(212, 154, 94, 0.06) 0%, rgba(15, 13, 12, 0) 70%);
  z-index: 1;
  pointer-events: none;
}

.hero-container {
  position: relative;
  z-index: 2;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 60px;
  width: 100%;
  max-width: 1300px;
  margin: 0 auto;
  padding: 0 40px;
}

.hero-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  max-width: 650px;
}

.hero-title {
  margin: 0 0 24px 0;
  font-size: 88px;
  font-weight: 600;
  color: #F9F6F0;
  line-height: 1.05;
  letter-spacing: -0.02em;
  white-space: normal;
  word-break: keep-all; 
}

.hero-description {
  margin: 0 0 48px 0;
  font-size: 20px;
  color: #B8A99A;
  line-height: 1.7;
}

/* ==========================================================================
   INTERACTIVE ELEMENTS (BUTTONS)
   ========================================================================== */
.hero-actions {
  display: flex;
  gap: 20px;
}

.btn-wrapper {
  display: inline-flex;
  padding-top: 4px;
  margin-top: -4px;
}

.btn {
  position: relative;
  top: 0px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 20px 40px;
  font-size: 13px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 2px;
  white-space: nowrap; 
  border-radius: 100px;
  cursor: pointer;
  text-decoration: none;
  transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1);
  will-change: top, background-color, box-shadow, border-color;
}

.btn-primary {
  background-color: #D49A5E;
  color: #0F0D0C;
  border: 1px solid #D49A5E;
}

.btn-wrapper:hover .btn-primary {
  background-color: #E2AD75;
  border-color: #E2AD75;
  top: -4px;
  box-shadow: 0 15px 30px rgba(212, 154, 94, 0.25);
}

.btn-outline {
  background-color: transparent;
  color: #F9F6F0;
  border: 1px solid rgba(249, 246, 240, 0.2);
}

.btn-wrapper:hover .btn-outline {
  border-color: #D49A5E;
  color: #D49A5E;
  top: -4px;
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2);
}

/* ==========================================================================
   PREMIUM FOOD VISUAL (PERCENTAGE-BASED TO AVOID SCALE/TRANSFORM)
   ========================================================================== */
.hero-visual {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  height: 650px;
}

.css-food-magic {
  position: relative;
  width: 500px;
  height: 500px;
  display: block;
}

.food-aura {
  position: absolute;
  width: 80%;
  height: 80%;
  left: 10%;
  top: 10%;
  background: radial-gradient(circle, rgba(212, 154, 94, 0.3) 0%, rgba(212, 154, 94, 0.05) 40%, rgba(15, 13, 12, 0) 70%);
  filter: blur(45px);
  z-index: 1;
  will-change: width, height, top, left, opacity;
  animation: pulseAura 4s ease-in-out infinite alternate;
}

.meal-composition {
  position: relative;
  width: 100%;
  height: 100%;
  z-index: 10;
  will-change: top;
  animation: floatMeal 6s ease-in-out infinite;
}

.plate {
  position: absolute;
  bottom: 16%;
  left: 50%;
  margin-left: -34%;
  width: 68%;
  height: 18%;
  border-radius: 50%;
  background: radial-gradient(ellipse at center, #F9F6F0 0%, #D8D1C5 100%);
  box-shadow: 
    0 25px 40px rgba(0,0,0,0.6), 
    inset 0 -5px 15px rgba(0,0,0,0.1),
    0 5px 0 #A8A196;
}

.plate-inner {
  position: absolute;
  top: 11%;
  bottom: 11%;
  left: 6%;
  right: 6%;
  border-radius: 50%;
  background: radial-gradient(ellipse at center, #EFECE5 0%, #E2DCD1 100%);
  box-shadow: inset 0 5px 10px rgba(0,0,0,0.15);
}

.pancake {
  position: absolute;
  left: 50%;
  margin-left: -20%;
  width: 40%;
  height: 15%;
  border-radius: 50%;
  background: radial-gradient(ellipse at center, #FADBAA 0%, #EAA25B 100%);
  box-shadow:
    inset -10px -10px 20px rgba(0,0,0,0.1),
    0 14px 0 #C27A34, 
    0 25px 30px rgba(0,0,0,0.4);
  will-change: margin-bottom;
}

.p1 { bottom: 22%; animation: floatLayer1 4s ease-in-out infinite alternate; }
.p2 { bottom: 31%; animation: floatLayer2 4s ease-in-out infinite alternate 0.3s; }
.p3 { bottom: 40%; animation: floatLayer3 4s ease-in-out infinite alternate 0.6s; }

.cream-blob {
  position: absolute;
  top: 13.3%;
  left: 50%;
  margin-left: -17.5%;
  width: 35%;
  height: 60%;
  background: #FFF;
  border-radius: 60% 40% 50% 50% / 50% 60% 40% 50%;
  box-shadow: 
    inset -5px -5px 10px rgba(0,0,0,0.1),
    0 5px 10px rgba(0,0,0,0.15);
}

.syrup-drop {
  position: absolute;
  background: #D49A5E;
  border-radius: 10px;
  box-shadow: inset -2px -2px 5px rgba(0,0,0,0.2);
}

.drop-1 { width: 7.5%; height: 46.6%; right: 15%; top: 53.3%; }
.drop-2 { width: 7.5%; height: 33.3%; left: 20%; top: 66.6%; }

.flying-ingredients {
  position: absolute;
  width: 100%;
  height: 100%;
  z-index: 20;
  pointer-events: none;
}

.leaf {
  position: absolute;
  width: 7%;
  height: 7%;
  border-radius: 50% 0 50% 0;
  background: linear-gradient(135deg, #2ECC71, #27AE60);
  box-shadow: 0 5px 15px rgba(39, 174, 96, 0.4);
  will-change: margin-top;
}
.mint-1 { top: 25%; left: 15%; animation: floatItem1 5s ease-in-out infinite alternate; }
.mint-2 { top: 60%; right: 10%; animation: floatItem2 4s ease-in-out infinite alternate-reverse; }

.berry {
  position: absolute;
  border-radius: 50%;
  box-shadow: inset -3px -3px 8px rgba(0,0,0,0.3), 0 10px 20px rgba(0,0,0,0.4);
  will-change: margin-top;
}
.red-berry { background: radial-gradient(circle at 30% 30%, #FF4E50, #C0392B); }
.blue-berry { background: radial-gradient(circle at 30% 30%, #5D9CEC, #3A539B); }
.b1 { width: 5%; height: 5%; top: 15%; right: 25%; animation: floatItem3 4.5s ease-in-out infinite alternate; }
.b2 { width: 4%; height: 4%; top: 40%; left: 5%; animation: floatItem4 6s ease-in-out infinite alternate-reverse; }
.b3 { width: 4.4%; height: 4.4%; bottom: 30%; right: 5%; animation: floatItem1 5.5s ease-in-out infinite alternate; }


/* ==========================================================================
   KEYFRAMES (STRICT TOP/BOTTOM/MARGIN INSTEAD OF TRANSFORM)
   ========================================================================== */
@keyframes floatMeal { 0% { top: 0%; } 50% { top: -3%; } 100% { top: 0%; } }
@keyframes pulseAura { 0% { width: 76%; height: 76%; top: 12%; left: 12%; opacity: 0.6; } 100% { width: 84%; height: 84%; top: 8%; left: 8%; opacity: 1; } }
@keyframes floatLayer1 { 0% { margin-bottom: 0%; } 100% { margin-bottom: -1.6%; } }
@keyframes floatLayer2 { 0% { margin-bottom: 0%; } 100% { margin-bottom: -2.4%; } }
@keyframes floatLayer3 { 0% { margin-bottom: 0%; } 100% { margin-bottom: -3.2%; } }
@keyframes floatItem1 { 0% { margin-top: 0%; } 100% { margin-top: -4%; } }
@keyframes floatItem2 { 0% { margin-top: 0%; } 100% { margin-top: -5%; } }
@keyframes floatItem3 { 0% { margin-top: 0%; } 100% { margin-top: -3%; } }
@keyframes floatItem4 { 0% { margin-top: 0%; } 100% { margin-top: 6%; } }

@keyframes fadeUp { 0% { opacity: 0; top: 40px; } 100% { opacity: 1; top: 0px; } }
@keyframes fadeInTop { 0% { opacity: 0; top: 20px; } 100% { opacity: 1; top: 0px; } }

.animate-title, .animate-desc, .animate-actions, .animate-visual {
  position: relative;
  opacity: 0;
  will-change: top, opacity;
}
.animate-title   { animation: fadeUp 1.2s cubic-bezier(0.16, 1, 0.3, 1) forwards 0.1s; }
.animate-desc    { animation: fadeUp 1.2s cubic-bezier(0.16, 1, 0.3, 1) forwards 0.3s; }
.animate-actions { animation: fadeUp 1.2s cubic-bezier(0.16, 1, 0.3, 1) forwards 0.5s; }
.animate-visual  { animation: fadeInTop 1.6s cubic-bezier(0.16, 1, 0.3, 1) forwards 0.6s; }

/* ==========================================================================
   RESPONSIVE DESIGN
   ========================================================================== */
@media (max-width: 992px) {
  .hero-container {
    flex-direction: column;
    justify-content: center;
    align-items: center; 
    text-align: center;
    padding: 0 24px;
  }
  
  .hero-content {
    align-items: center;
    justify-content: center;
    max-width: 100%;
    margin-top: 0; 
  }
  
  .hero-title {
    font-size: 56px;
    text-align: center;
  }
  
  .hero-description {
    max-width: 500px;
    margin-bottom: 40px;
    text-align: center; 
  }

  .hero-actions {
    justify-content: center;
    align-items: center;
    width: 100%;
  }

  .hero-visual {
    display: block; 
    position: relative; 
    width: 100%;
    height: 180px; 
    margin-top: 0px; 
  }

  .css-food-magic {
    position: absolute; 
    top: 50%;
    left: 50%;
    margin-left: -137.5px;
    margin-top: -68.75px;
    width: 275px;
    height: 275px; 
  }
}

@media (max-width: 576px) {
  .hero-container {
    padding: 0 20px;
  }

  .hero-title {
    font-size: 38px; 
    word-break: normal; 
  }
  
  .hero-actions {
    flex-direction: column;
    width: 100%;
    gap: 16px;
  }

  .btn-wrapper {
    width: 100%;
    display: flex;
    justify-content: center;
  }

  .btn {
    width: 100%;
    padding: 18px 16px;
    font-size: 11.5px;
    justify-content: center;
    text-align: center;
  }

  .hero-visual {
    height: 150px; 
    margin-top: 0px; 
  }
  
  .css-food-magic {
    margin-left: -105px;
    margin-top: -52.5px;
    width: 210px;
    height: 210px; 
  }
}

@media (max-width: 576px) and (max-height: 700px) {
  .hero-visual {
    display: none !important;
  }
}
</style>