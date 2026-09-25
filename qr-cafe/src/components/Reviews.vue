<template>
  <section id="reviews" class="reviews-section">
    <div class="reviews-container">
      
      <!-- ==========================================================================
           HEADER SECTION
           ========================================================================== -->
      <div class="section-header">
        <span class="subtitle animate-on-scroll fade-up">Репутація</span>
        <h2 class="title animate-on-scroll fade-up delay-100">
          Що кажуть наші гості
        </h2>
      </div>

      <!-- ==========================================================================
           REVIEWS GRID (STATIC)
           ========================================================================== -->
      <div class="reviews-grid">
        <article 
          v-for="(review, index) in staticReviews" 
          :key="index" 
          class="review-card animate-on-scroll fade-up"
          :style="{ transitionDelay: `${index * 0.15}s` }"
        >
          <div class="stars">
            <svg v-for="i in 5" :key="`star-${index}-${i}`" class="star-icon" viewBox="0 0 24 24" fill="currentColor">
              <path d="M12 17.27L18.18 21l-1.64-7.03L22 9.24l-7.19-.61L12 2 9.19 8.63 2 9.24l5.46 4.73L5.82 21z"/>
            </svg>
          </div>
          
          <p class="review-text">«{{ review.text }}»</p>
          
          <div class="review-author">
            <span class="author-name">{{ review.name }}</span>
            <span class="author-project">{{ review.project }}</span>
          </div>
        </article>
      </div>

      <!-- ==========================================================================
           CALL TO ACTION BUTTON (GOOGLE REVIEWS)
           ========================================================================== -->
      <div class="reviews-action">
        <div class="btn-wrapper animate-on-scroll fade-up delay-300">
          <a href="https://www.google.com/maps/place/Qr+%D0%A1afe/@46.4825475,30.593379,18z/data=!4m8!3m7!1s0x40c62d4359e36e5d:0x5533e0147e4710f6!8m2!3d46.4825457!4d30.5946691!9m1!1b1!16s%2Fg%2F11vf51z_r8" target="_blank" rel="noopener noreferrer" class="btn-google">
            <svg class="google-icon" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
              <path d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z" fill="#4285F4"/>
              <path d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z" fill="#34A853"/>
              <path d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z" fill="#FBBC05"/>
              <path d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z" fill="#EA4335"/>
            </svg>
            Читати всі відгуки на Google
          </a>
        </div>
      </div>

    </div>
  </section>
</template>

<script setup>
import { onMounted, onUnmounted } from 'vue';

// Top 3 static reviews
const staticReviews = [
  { 
    name: "Анна О", 
    project: "Місцевий експерт", 
    text: "Безперечно 5 зірок. Були ввечері та вранці. Чудова домашня кухня. Великі порції та швидка подача. Грецький салат дуже смачний, печеня в горщику – смакота!!!" 
  },
  { 
    name: "Марія Ель Хавван", 
    project: "Місцевий експерт", 
    text: "Все супер, дуже смачно, чисто! Порції великі, ціни адекватні. Привітний персонал. Якщо буду тут знову, обов'язково прийду 😍" 
  },
  { 
    name: "Jack Black", 
    project: "Місцевий експерт", 
    text: "Дуже навіть! Прекрасне місце. Смачно готують, порції великі, смак як вдома, адекватні ціни. Рекомендуємо." 
  }
];

let observer = null;

onMounted(() => {
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
  if (observer) observer.disconnect();
});
</script>

<style scoped>
/* ==========================================================================
   BASE SECTION STYLES
   ========================================================================== */
.reviews-section {
  width: 100%;
  padding: 120px 0;
  background-color: #161311; 
  color: #F9F6F0; 
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.reviews-container {
  width: 100%;
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 40px;
}

/* ==========================================================================
   HEADER SECTION
   ========================================================================== */
.section-header {
  margin-bottom: 80px;
  text-align: center;
}

.subtitle {
  display: block;
  margin-bottom: 20px;
  font-size: 14px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 2.8px;
  color: #D49A5E; 
}

.title {
  margin: 0;
  font-size: 56px;
  font-weight: 500;
  line-height: 1.15;
  letter-spacing: -1.12px;
  color: #F9F6F0; 
}

/* ==========================================================================
   REVIEWS GRID AND CARDS (STATIC, LIGHT)
   ========================================================================== */
.reviews-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 32px;
  margin-bottom: 64px;
}

.review-card {
  position: relative;
  top: 0px;
  display: flex;
  flex-direction: column;
  padding: 48px 40px;
  background: linear-gradient(145deg, rgba(255, 255, 255, 0.08), rgba(255, 255, 255, 0.02)); 
  border: 1px solid rgba(212, 154, 94, 0.3); 
  border-radius: 24px;
  backdrop-filter: blur(24px);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2); 
  transition: top 0.5s cubic-bezier(0.16, 1, 0.3, 1), border-color 0.5s ease, box-shadow 0.5s ease;
  will-change: top, border-color, box-shadow;
}

.review-card.is-revealed:hover {
  background: linear-gradient(145deg, rgba(255, 255, 255, 0.12), rgba(255, 255, 255, 0.04)); 
  border-color: rgba(212, 154, 94, 0.7); 
  top: -8px;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.4); 
}

.stars {
  display: flex;
  gap: 4px;
  margin-bottom: 24px;
}

.star-icon {
  width: 20px;
  height: 20px;
  color: #D49A5E; 
}

.review-text {
  flex-grow: 1;
  margin: 0 0 40px 0;
  font-size: 17.6px;
  font-weight: 400; 
  line-height: 1.7;
  color: #F9F6F0; 
}

.review-author {
  display: flex;
  flex-direction: column;
  gap: 4px;
  padding-top: 24px;
  border-top: 1px dashed rgba(255, 255, 255, 0.2); 
}

.author-name {
  font-size: 17.6px;
  font-weight: 500;
  color: #FFFFFF; 
}

.author-project {
  font-size: 13.6px;
  color: #D49A5E; 
}

/* ==========================================================================
   CALL TO ACTION BUTTON (GOOGLE REVIEWS)
   ========================================================================== */
.reviews-action {
  display: flex;
  justify-content: center;
}

.btn-wrapper {
  display: inline-flex;
  margin-top: -4px;
  padding-top: 4px;
}

.btn-google {
  position: relative;
  top: 0px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 12px;
  padding: 16px 32px;
  font-size: 15.2px;
  font-weight: 500;
  text-decoration: none;
  color: #0F0D0C; 
  background-color: #F9F6F0; 
  border: 1px solid transparent; 
  border-radius: 100px; 
  transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1);
  will-change: top, background-color, box-shadow;
}

.google-icon {
  width: 24px;
  height: 24px;
}

.btn-wrapper:hover .btn-google {
  background-color: #ffffff; 
  top: -4px;
  box-shadow: 0 15px 30px rgba(255, 255, 255, 0.15);
}

/* ==========================================================================
   SCROLL REVEAL ANIMATIONS
   ========================================================================== */
.animate-on-scroll {
  position: relative;
  opacity: 0;
  transition: opacity 1.2s cubic-bezier(0.16, 1, 0.3, 1), top 1.2s cubic-bezier(0.16, 1, 0.3, 1) !important;
}

.fade-up { top: 40px; }

.animate-on-scroll.is-revealed {
  opacity: 1;
  top: 0px;
}

.delay-100 { transition-delay: 0.1s !important; }
.delay-300 { transition-delay: 0.3s !important; }

/* ==========================================================================
   RESPONSIVE DESIGN
   ========================================================================== */
@media (max-width: 1024px) {
  .reviews-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 768px) {
  .reviews-section {
    padding: 80px 0;
  }
  
  .reviews-container {
    padding: 0 24px;
  }
  
  .title {
    font-size: 38px;

  }

  .reviews-grid {
    grid-template-columns: 1fr;
    gap: 24px;
  }

  .review-card {
    padding: 32px 24px;
    transition-delay: 0s !important;
  }
  
  .btn-wrapper, .btn-google {
    width: 100%;
  }
}

@media (max-width: 480px) {
  .title {
    font-size: 26px; 
  }

  .review-card {
    padding: 24px 20px;
  }

  .review-text {
    margin-bottom: 24px;
    font-size: 16px; 
  }

  .btn-google {
    gap: 8px; 
    padding: 16px 0; 
    font-size: 11.5px;
    white-space: nowrap;
  }

  .google-icon {
    width: 20px;
    height: 20px;
  }
}
</style>