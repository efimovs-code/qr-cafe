<template>
  <footer id="footer" class="footer-section">
    <div class="footer-container">
      
      <!-- ==========================================================================
           CALL TO ACTION (CTA)
           ========================================================================== -->
      <div class="footer-cta-wrapper">
        <h2 class="footer-title animate-on-scroll fade-up">
          Ваш ідеальний вечір <br/> починається з бронювання.
        </h2>
        
        <p class="footer-description animate-on-scroll fade-up delay-100">
          Зв'яжіться з нами безпосередньо або забронюйте столик за пару кліків.
        </p>

        <!-- PREMIUM BUTTON -->
        <div class="btn-wrapper animate-on-scroll fade-up delay-200">
          <button class="btn-cta" @click="openModal">
            <span>Забронювати стіл</span>
          </button>
        </div>
        
        <!-- ==========================================================================
             CONTACT LINKS (INLINE) & GOOGLE MAP
             ========================================================================== -->
        <div class="footer-contacts-inline animate-on-scroll fade-up delay-300">
          <a href="tel:+380961805976" class="minimal-link">+380 (96) 180-59-76</a>
          
          <!-- COPY ADDRESS BUTTON WITH STATUS UPDATE -->
          <button 
            @click="handleAddressClick" 
            class="minimal-link btn-reset" 
            :class="{ 'text-success': isAddressCopied }"
          >
            {{ isAddressCopied ? 'Адресу скопійовано!' : 'Хлібодарське, вул. Покровська' }}
          </button>
        </div>

        <!-- INTEGRATED GOOGLE MAPS IFRAME -->
        <div class="footer-map-wrapper animate-on-scroll fade-up delay-300">
          <iframe 
            src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d2748.2435532555!2d30.59209417684824!3d46.48254946618428!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x40c62d4359e36e5d%3A0x5533e0147e4710f6!2zUXIg0KHQsGZl!5e0!3m2!1sru!2sua!4v1716385555555!5m2!1sru!2sua" 
            width="100%" 
            height="100%" 
            style="border:0;" 
            allowfullscreen="" 
            loading="lazy" 
            referrerpolicy="no-referrer-when-downgrade">
          </iframe>
        </div>

      </div>

    </div>

    <!-- ==========================================================================
         BOTTOM PANEL & COPYRIGHT
         ========================================================================== -->
    <div class="footer-bottom">
      <span class="copyright">© 2026 Qr Cafe. Всі права захищені.</span>
      
      <!-- EFIMOVS AGENCY BRAND BADGE WRAPPER FOR STRICT OVERRIDE -->
      <div class="badge-wrapper">
        <EfimovsBadge />
      </div>
    </div>

    <!-- MODAL TELEPORT -->
    <ContactModal :isOpen="isModalOpen" @close="isModalOpen = false" />
  </footer>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import EfimovsBadge from './EfimovsBadge.vue'; 
import ContactModal from './ContactModal.vue'; 

const isModalOpen = ref(false);
const isAddressCopied = ref(false); 

const openModal = () => {
  isModalOpen.value = true;
};

// COPY TO CLIPBOARD HANDLER WITH NOTIFICATION RESET
const handleAddressClick = async () => {
  const address = 'Хлібодарське, вул. Покровська, 5а';
  
  try {
    await navigator.clipboard.writeText(address);
    isAddressCopied.value = true;
    
    setTimeout(() => {
      isAddressCopied.value = false;
    }, 2000);
  } catch (err) {
    console.error('Copy error: ', err);
  }
};

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
   BASE LAYOUT
   ========================================================================== */
.footer-section {
  position: relative;
  width: 100%;
  padding: 160px 0 40px;
  background-color: #0F0D0C; 
  color: #F9F6F0;
  overflow: hidden;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.footer-container {
  position: relative;
  z-index: 10;
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 40px;
  text-align: center;
}

/* ==========================================================================
   CALL TO ACTION (CTA)
   ========================================================================== */
.footer-cta-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
}

.footer-title {
  margin: 0 0 32px 0;
  font-size: 64px;
  font-weight: 500;
  line-height: 1.05;
  color: #F9F6F0;
  letter-spacing: -1.92px;
}

.footer-description {
  max-width: 600px;
  margin: 0 0 40px 0; 
  font-size: 18.4px;
  line-height: 1.6;
  color: #B8A99A;
}

/* --- BUTTON STYLES --- */
.btn-wrapper {
  display: inline-flex;
  margin: -4px 0 64px 0; 
  padding-top: 4px;
}

.btn-cta {
  position: relative;
  top: 0px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 20px 56px;
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
  will-change: top, background-color, box-shadow;
}

.btn-wrapper:hover .btn-cta {
  background-color: #E2AD75;
  top: -4px;
  box-shadow: 0 15px 30px rgba(212, 154, 94, 0.25);
}

/* ==========================================================================
   CONTACT LINKS (INLINE)
   ========================================================================== */
.footer-contacts-inline {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  gap: 56px;
  margin-bottom: 50px;
}

.btn-reset {
  padding: 0;
  font-family: inherit;
  background: transparent;
  border: none;
  outline: none;
  appearance: none;
  cursor: pointer;
}

.minimal-link {
  position: relative;
  font-size: 24px;
  font-weight: 500;
  color: #F9F6F0;
  text-decoration: none;
  letter-spacing: -0.24px;
  transition: color 0.4s ease;
}

.minimal-link::after {
  content: '';
  position: absolute;
  bottom: -4px;
  left: 0;
  width: 0px;
  height: 2px;
  background-color: #D49A5E;
  transition: width 0.4s cubic-bezier(0.16, 1, 0.3, 1), background-color 0.4s ease;
}

.minimal-link:hover {
  color: #D49A5E;
}

.minimal-link:hover::after {
  width: 100%;
}

.minimal-link.text-success {
  color: #4ade80; 
}

.minimal-link.text-success::after {
  width: 100%;
  background-color: #4ade80;
}

/* ==========================================================================
   GOOGLE MAP EMBED STYLES
   ========================================================================== */
.footer-map-wrapper {
  width: 100%;
  max-width: 900px;
  height: 380px;
  border-radius: 24px;
  overflow: hidden;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.4);
  border: 1px solid rgba(212, 154, 94, 0.15);
  filter: contrast(1.1) brightness(0.9) sepia(0.1);
  transition: filter 0.3s ease;
}

.footer-map-wrapper:hover {
  filter: contrast(1) brightness(1) sepia(0);
}

/* ==========================================================================
   BOTTOM PANEL & WATERMARK FIX
   ========================================================================== */
.footer-bottom {
  position: relative;
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  max-width: 1300px;
  margin: 120px auto 0;
  padding: 0 40px;
  box-sizing: border-box;
}

.copyright {
  font-size: 13.6px;
  color: #B8A99A; 
}

/* 
   STRICT EFIMOVS AGENCY BADGE FIX:
   FORCE WHITE COLOR AND REMOVE ALL TRANSPARENCY OVERRIDES
*/
.badge-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 1 !important;
}

.badge-wrapper :deep([style]) {
  opacity: 1 !important;
}

.badge-wrapper :deep(span),
.badge-wrapper :deep(p),
.badge-wrapper :deep(div),
.badge-wrapper :deep(a) {
  color: #FFFFFF !important;
  opacity: 1 !important;
}

.badge-wrapper :deep(svg),
.badge-wrapper :deep(path),
.badge-wrapper :deep(g),
.badge-wrapper :deep(rect) {
  fill: #FFFFFF !important;
  color: #FFFFFF !important;
  opacity: 1 !important;
}

.badge-wrapper :deep(svg path[stroke]),
.badge-wrapper :deep(svg rect[stroke]) {
  stroke: #FFFFFF !important;
}

.badge-wrapper :deep(img) {
  filter: brightness(0) invert(1) !important;
  opacity: 1 !important;
}

/* ==========================================================================
   SCROLL REVEAL ANIMATIONS (STRICT TOP/PX INSTEAD OF TRANSFORM)
   ========================================================================== */
.animate-on-scroll {
  position: relative;
  opacity: 0;
  transition: opacity 1.2s cubic-bezier(0.16, 1, 0.3, 1), top 1.2s cubic-bezier(0.16, 1, 0.3, 1) !important;
  will-change: top, opacity;
}

.fade-up { top: 40px; }

.animate-on-scroll.is-revealed {
  opacity: 1;
  top: 0px;
}

.delay-100 { transition-delay: 0.1s !important; }
.delay-200 { transition-delay: 0.2s !important; }
.delay-300 { transition-delay: 0.3s !important; } 

/* ==========================================================================
   RESPONSIVE DESIGN
   ========================================================================== */
@media (max-width: 992px) {
  .footer-title {
    font-size: 51.2px;
  }
  
  .footer-contacts-inline {
    flex-direction: column;
    gap: 24px;
  }

  .footer-map-wrapper {
    height: 300px;
  }
}

@media (max-width: 768px) {
  .footer-section {
    padding: 100px 0 32px;
  }
  
  .footer-container {
    padding: 0 24px;
  }

  .footer-title {
    margin-bottom: 24px;
    font-size: 38.4px;
  }

  .footer-description {
    margin-bottom: 32px;
    font-size: 16.8px;
  }

  .btn-wrapper {
    width: 100%;
    margin-bottom: 48px;
  }

  .btn-cta {
    width: 100%;
    padding: 20px 0; 
  }

  .minimal-link {
    font-size: 20px;
  }

  .footer-map-wrapper {
    height: 240px;
  }

  .footer-bottom {
    flex-direction: column;
    gap: 32px;
    margin-top: 80px;
    text-align: center;
  }
}

@media (max-width: 480px) {
  .footer-section {
    padding: 80px 0 32px;
  }

  .footer-title {
    font-size: 28px; 
    white-space: nowrap; 
  }

  .footer-description {
    padding: 0 10px;
    margin-bottom: 32px;
    font-size: 15px;
  }

  .btn-wrapper {
    margin-bottom: 40px;
  }

  .btn-cta {
    font-size: 11.5px;
    white-space: nowrap;
  }

  .footer-contacts-inline {
    gap: 20px; 
  }

  .minimal-link {
    font-size: 18px; 
  }

  .footer-bottom {
    margin-top: 60px; 
    gap: 24px;
  }
}
</style>