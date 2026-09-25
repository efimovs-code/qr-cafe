<template>
  <section id="expertise" class="expertise-presentation">
    
    <!-- ==========================================================================
         HEADER SECTION
         ========================================================================== -->
    <div class="expertise-intro bg-dark">
      <div class="intro-container">
        <span class="subtitle animate-on-scroll fade-up">Чому нас обирають</span>
        <h2 class="title animate-on-scroll fade-up delay-100">
          Атмосфера, в яку<br/>хочеться повертатися
        </h2>
      </div>
    </div>

    <!-- ==========================================================================
         SLIDE 01: ДОМАШНЯ КУХНЯ
         ========================================================================== -->
    <article class="expertise-slide bg-dark">
      <div class="slide-content">
        <div class="content-inner">
          <span class="slide-number" aria-hidden="true">01</span>
          <h2 class="slide-title animate-on-scroll fade-up delay-100">
            Домашня кухня<br/>та великі порції
          </h2>
          <p class="slide-description animate-on-scroll fade-up delay-200">
            Готуємо з душею. Ті самі сирники на сніданок та печеня в горщику на вечерю, про які пишуть у відгуках. Бездоганний смак та щедра подача.
          </p>
          <button class="slide-action animate-on-scroll fade-up delay-300" @click="openModal">
            <span class="action-text">Забронювати</span>
            <svg class="action-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
              <path d="M5 12h14M12 5l7 7-7 7" stroke-width="1.5" stroke-linecap="square" stroke-linejoin="miter"/>
            </svg>
          </button>
        </div>
      </div>
      <div class="slide-visual">
        <!-- Замінено scale-up на fade-up для консистентності з новими правилами анімації -->
        <div class="image-wrapper slider-wrapper animate-on-scroll fade-up delay-200">
          <img 
            v-for="(img, index) in foodImages" 
            :key="`food-${index}`"
            :src="img" 
            alt="Домашня кухня" 
            class="slide-image slider-image" 
            :class="{ 'is-active': currentFoodIndex === index }"
          />
        </div>
      </div>
    </article>

    <!-- ==========================================================================
         SLIDE 02: ДОСТАВКА
         ========================================================================== -->
    <article class="expertise-slide reverse bg-darker">
      <div class="slide-content">
        <div class="content-inner">
          <span class="slide-number" aria-hidden="true">02</span>
          <h2 class="slide-title animate-on-scroll fade-up delay-100">
            Швидка доставка<br/>та їжа навинос
          </h2>
          <p class="slide-description animate-on-scroll fade-up delay-200">
            Немає часу сидіти в кафе? Ми приготуємо та дбайливо спакуємо ваш улюблений обід із собою або доставимо його гарячим прямо до ваших дверей.
          </p>
          <button class="slide-action animate-on-scroll fade-up delay-300" @click="openModal">
            <span class="action-text">Зробити замовлення</span>
            <svg class="action-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
              <path d="M5 12h14M12 5l7 7-7 7" stroke-width="1.5" stroke-linecap="square" stroke-linejoin="miter"/>
            </svg>
          </button>
        </div>
      </div>
      <div class="slide-visual">
        <div class="image-wrapper slider-wrapper animate-on-scroll fade-up delay-200">
          <img 
            v-for="(img, index) in deliveryImages" 
            :key="`delivery-${index}`"
            :src="img" 
            alt="Доставка" 
            class="slide-image slider-image" 
            :class="{ 'is-active': currentDeliveryIndex === index }"
          />
        </div>
      </div>
    </article>

    <!-- ==========================================================================
         SLIDE 03: БАР ТА АТМОСФЕРА
         ========================================================================== -->
    <article class="expertise-slide bg-dark">
      <div class="slide-content">    
        <div class="content-inner">
          <span class="slide-number" aria-hidden="true">03</span>
          <h2 class="slide-title animate-on-scroll fade-up delay-100">
            Відмінна кава<br/>та бар
          </h2>
          <p class="slide-description animate-on-scroll fade-up delay-200">
            Почніть день з правильної спешелті кави, а вечір — з келиха вина або крафтового пива. Ідеальне місце для зустрічей з друзями та відпочинку з сім'єю.
          </p>
          <button class="slide-action animate-on-scroll fade-up delay-300" @click="openModal">
            <span class="action-text">Забронювати</span>
            <svg class="action-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
              <path d="M5 12h14M12 5l7 7-7 7" stroke-width="1.5" stroke-linecap="square" stroke-linejoin="miter"/>
            </svg>
          </button>
        </div>
      </div>
      <div class="slide-visual">
        <div class="image-wrapper animate-on-scroll fade-up delay-200">
          <img src="https://images.unsplash.com/photo-1497935586351-b67a49e012bf?q=80&w=1000&auto=format&fit=crop" alt="Кава та бар" class="slide-image" />
        </div>
      </div>
    </article>

    <!-- ==========================================================================
         FINAL CTA BLOCK
         ========================================================================== -->
    <div class="expertise-cta bg-darker">
      <div class="cta-container">
        <h2 class="cta-title animate-on-scroll fade-up">
          Ваш ідеальний вечір <br/> починається тут
        </h2>
        <div class="btn-wrapper animate-on-scroll fade-up delay-100">
          <button class="btn-cta" @click="openModal">
            <span>Забронювати стіл</span>
          </button>
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
import { ref, onMounted, onUnmounted } from 'vue';
import ContactModal from './ContactModal.vue';

const foodImages = [
  'https://images.unsplash.com/photo-1565299624946-b28f40a0ae38?q=80&w=1000&h=1500&auto=format&fit=crop', 
  'https://images.unsplash.com/photo-1555939594-58d7cb561ad1?q=80&w=1000&h=1500&auto=format&fit=crop'  
];

const deliveryImages = [
  'https://images.unsplash.com/photo-1490645935967-10de6ba17061?q=80&w=1000&h=1500&auto=format&fit=crop', 
  'https://images.unsplash.com/photo-1584916201218-f4242ceb4809?q=80&w=1000&h=1500&auto=format&fit=crop'  
];

const isModalOpen = ref(false);
const currentFoodIndex = ref(0);
const currentDeliveryIndex = ref(0);

let foodInterval = null;
let deliveryInterval = null;
let observer = null; 

const openModal = () => {
  isModalOpen.value = true;
};

onMounted(() => {
  foodInterval = setInterval(() => {
    currentFoodIndex.value = (currentFoodIndex.value + 1) % foodImages.length;
  }, 4000);

  deliveryInterval = setInterval(() => {
    currentDeliveryIndex.value = (currentDeliveryIndex.value + 1) % deliveryImages.length;
  }, 4500);

  observer = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        entry.target.classList.add('is-revealed');
        observer.unobserve(entry.target); 
      }
    });
  }, {
    threshold: 0.15, 
    rootMargin: "0px 0px -50px 0px"
  });

  document.querySelectorAll('.animate-on-scroll').forEach((el) => {
    observer.observe(el);
  });
});

onUnmounted(() => {
  if (foodInterval) clearInterval(foodInterval);
  if (deliveryInterval) clearInterval(deliveryInterval);
  if (observer) observer.disconnect(); 
});
</script>

<style scoped>
/* ==========================================================================
   BASE LAYOUT & TYPOGRAPHY
   ========================================================================== */
.expertise-presentation {
  display: flex;
  flex-direction: column;
  width: 100%;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* Преміальна темна палітра */
.bg-dark {
  background-color: #0F0D0C;
}

.bg-darker {
  background-color: #161311;
}

/* ==========================================================================
   SECTION INTRO
   ========================================================================== */
.expertise-intro {
  display: flex;
  justify-content: center;
  width: 100%;
  padding: 80px 0 60px;
  text-align: center;
}

.intro-container {
  width: 100%;
  max-width: 1400px;
  padding: 0 40px;
}

.subtitle {
  display: block;
  margin-bottom: 24px;
  font-size: 14px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 3px;
  color: #D49A5E; /* Карамельний акцент */
}

.title {
  margin: 0;
  font-size: 56px; 
  font-weight: 600;
  color: #F9F6F0; /* Кремово-білий */
  line-height: 1.15; 
  letter-spacing: -1px; 
}

/* ==========================================================================
   SLIDE LAYOUT
   ========================================================================== */
.expertise-slide {
  position: relative;
  display: flex;
  align-items: center; 
  width: 100%;
  height: 720px;
  padding: 40px 0; 
  overflow: hidden;
}

.expertise-slide.reverse {
  flex-direction: row-reverse;
}

/* ==========================================================================
   CONTENT AREA (35%)
   ========================================================================== */
.slide-content {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 35%;
}

.content-inner {
  position: relative;
  z-index: 10;
  max-width: 600px;
  padding: 0 40px;
}

.slide-number {
  position: absolute;
  top: -80px;
  left: 0px;
  font-size: 192px;
  font-weight: 700;
  color: #ffffff;
  opacity: 0.03; /* Ледь помітна біла цифра на темному тлі */
  line-height: 1;
  pointer-events: none;
  user-select: none;
}

.slide-title {
  position: relative;
  margin: 0 0 32px 0;
  font-size: 64px;
  font-weight: 600;
  color: #F9F6F0;
  line-height: 1.1;
  letter-spacing: -1px;
}

.slide-description {
  position: relative;
  margin: 0 0 48px 0;
  font-size: 20px;
  color: #B8A99A; /* Теплий сірий */
  line-height: 1.7;
}

.slide-action {
  position: relative;
  display: inline-flex;
  align-items: center;
  gap: 16px;
  padding: 0;
  color: #D49A5E;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}

.action-text {
  font-size: 16px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 2px;
  transition: color 0.3s ease;
}

.action-icon {
  position: relative;
  left: 0px;
  width: 24px;
  height: 24px;
  transition: left 0.4s cubic-bezier(0.16, 1, 0.3, 1);
  will-change: left;
}

.slide-action:hover .action-icon {
  left: 8px;
}

.slide-action:hover .action-text {
  color: #E2AD75;
}

/* ==========================================================================
   VISUAL AREA (65%)
   ========================================================================== */
.slide-visual {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 65%;
  height: 100%; 
  padding: 0 20px; 
}

.image-wrapper {
  position: relative;
  display: inline-flex; 
  max-width: 100%;
  max-height: 100%;
  overflow: hidden;
  border-radius: 24px; 
  box-shadow: 0 30px 60px rgba(0, 0, 0, 0.5); /* Глибока тінь */
}

.slider-wrapper {
  display: grid;
  place-items: center; /* Центруємо контент, щоб розширення (width %) йшло від центру */
}

.slide-image {
  display: block;
  width: 100%; 
  max-width: 100%;
  height: auto; 
  max-height: 640px; 
  object-fit: contain; 
  transition: width 1.5s cubic-bezier(0.16, 1, 0.3, 1), max-width 1.5s cubic-bezier(0.16, 1, 0.3, 1);
  will-change: width;
}

.slider-image {
  grid-area: 1 / 1; 
  opacity: 0;
  transition: opacity 1.5s ease-in-out, width 1.5s cubic-bezier(0.16, 1, 0.3, 1), max-width 1.5s cubic-bezier(0.16, 1, 0.3, 1);
  will-change: opacity, width;
}

.slider-image.is-active {
  opacity: 1;
  z-index: 2;
}

.expertise-slide:hover .slide-image {
  width: 103%;
  max-width: 103%;
}

/* ==========================================================================
   FINAL CTA SECTION
   ========================================================================== */
.expertise-cta {
  position: relative;
  z-index: 20; 
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  margin-top: 0; 
  padding: 100px 0 140px; 
  text-align: center;
}

.cta-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  max-width: 800px;
  padding: 0 40px;
}

.cta-title {
  margin: 0 0 48px 0;
  font-size: 40px;
  font-weight: 500;
  color: #F9F6F0;
  letter-spacing: -1px;
}

.btn-wrapper {
  display: inline-flex;
  margin-top: -4px;
  padding-top: 4px;
}

.btn-cta {
  position: relative;
  top: 0px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 18px 48px; 
  font-size: 13px; 
  font-weight: 700; 
  text-transform: uppercase;
  letter-spacing: 2px;
  color: #0F0D0C; 
  background-color: #D49A5E; 
  border: none;
  border-radius: 100px; 
  cursor: pointer;
  transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}

.btn-wrapper:hover .btn-cta {
  background-color: #E2AD75;
  top: -4px;
  box-shadow: 0 15px 30px rgba(212, 154, 94, 0.25);
}

/* ==========================================================================
   SCROLL REVEAL ANIMATIONS
   ========================================================================== */
.animate-on-scroll {
  position: relative;
  opacity: 0;
  transition: opacity 1.2s cubic-bezier(0.16, 1, 0.3, 1), top 1.2s cubic-bezier(0.16, 1, 0.3, 1);
  will-change: top, opacity;
}

.fade-up { top: 40px; }

.animate-on-scroll.is-revealed {
  opacity: 1;
  top: 0px;
}

.delay-100 { transition-delay: 0.1s; }
.delay-200 { transition-delay: 0.2s; }
.delay-300 { transition-delay: 0.3s; }

/* ==========================================================================
   RESPONSIVE DESIGN
   ========================================================================== */
@media (max-width: 1024px) {
  .expertise-intro {
    padding: 30px 0 40px;
  }
  .title {
    font-size: 51px; 
  }
  .slide-title {
    font-size: 48px;
  }
  .slide-description {
    font-size: 18px;
  }
  .slide-visual {
    padding: 0 20px;
  }
}

@media (max-width: 992px) {
  .expertise-slide, .expertise-slide.reverse {
    flex-direction: column;
    height: auto;
    padding: 40px 0;
  }

  .slide-content, .slide-visual {
    width: 100%;
  }

  .slide-content {
    padding: 80px 24px;
  }

  .content-inner {
    max-width: 100%;
    padding: 0;
  }

  .slide-visual {
    height: 540px; 
    padding: 0 24px;
  }
  
  .slide-image {
    max-height: 540px; 
  }

  .slide-number {
    top: -40px;
    font-size: 128px;
  }
  
  .expertise-cta {
    padding: 80px 0 100px; 
  }
}

@media (max-width: 768px) {
  .expertise-intro {
    padding: 40px 0 20px; 
  }
  .intro-container {
    padding: 0 24px;
  }
  .title {
    font-size: 45px; 
  }
  .cta-title {
    margin-bottom: 32px;
    font-size: 32px;
  }
  
  .btn-wrapper, .btn-cta {
    width: 100%;
  }
}

@media (max-width: 480px) {
  .slide-content {
    padding: 60px 24px;
  }

  .title {
    font-size: 30px; 
    white-space: nowrap; 
  }

  .slide-title {
    font-size: 40px;
  }
  .slide-description {
    font-size: 16px;
  }
  .slide-visual {
    height: 420px; 
    padding: 0 16px;
  }
  
  .slide-image {
    max-height: 420px; 
  }

  .image-wrapper {
    border-radius: 16px; 
  }
  .expertise-cta {
    padding: 60px 24px 80px; 
  }
  .cta-title {
    font-size: 26px; 
    white-space: nowrap; 
  }
  .btn-cta {
    white-space: nowrap;
  }
}
</style>