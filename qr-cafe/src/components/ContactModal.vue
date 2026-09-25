<template>
  <Transition name="modal-fade">
    <div v-if="isOpen" class="modal-overlay" @click.self="closeModal" data-lenis-prevent>
      
      <div class="modal-container">
        <button class="btn-close" @click="closeModal" aria-label="Close">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
            <path d="M18 6L6 18M6 6l12 12" stroke-width="1.5" stroke-linecap="square"/>
          </svg>
        </button>

        <div class="modal-header">
          <span class="subtitle">Booking</span>
          <h2 class="title">Забронювати стіл</h2>
          <p class="description">
            Оберіть зручний для вас спосіб зв'язку.
            <span class="desktop-only-text">
              <br/>Ми швидко відповімо та підберемо найкращий столик.
            </span>
          </p>
        </div>

        <div class="contact-links">
          
          <a href="tel:+380961805976" class="contact-card">
            <div class="icon-wrapper">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
                <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z" stroke-width="1.5" stroke-linecap="square"/>
              </svg>
            </div>
            <div class="contact-info">
              <span class="contact-label">Зателефонувати</span>
              <span class="contact-value">+380 (96) 180-59-76</span>
            </div>
          </a>

          <a href="viber://chat?number=%2B380961805976" class="contact-card">
            <div class="icon-wrapper">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
                <path d="M21 11.5a8.38 8.38 0 0 1-.9 3.8 8.5 8.5 0 0 1-7.6 4.7 8.38 8.38 0 0 1-3.8-.9L3 21l1.9-5.7a8.38 8.38 0 0 1-.9-3.8 8.5 8.5 0 0 1 4.7-7.6 8.38 8.38 0 0 1 3.8-.9h.5a8.48 8.48 0 0 1 8 8v.5z" stroke-width="1.5" stroke-linecap="square"/>
              </svg>
            </div>
            <div class="contact-info">
              <span class="contact-label">Написати у Viber</span>
              <span class="contact-value">Швидка відповідь</span>
            </div>
          </a>

          <button @click="handleAddressClick" class="contact-card btn-reset">
            <div class="icon-wrapper">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
                <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z" stroke-width="1.5" stroke-linecap="square"/>
                <circle cx="12" cy="10" r="3" stroke-width="1.5" stroke-linecap="square"/>
              </svg>
            </div>
            <div class="contact-info">
              <span class="contact-label" :class="{ 'text-success': isAddressCopied }">
                {{ isAddressCopied ? 'Адресу скопійовано!' : 'Як до нас доїхати' }}
              </span>
              <span class="contact-value address-value">Хлібодарське, вул. Покровська, 5а</span>
            </div>
          </button>

        </div>
      </div>
      
    </div>
  </Transition>
</template>

<script setup>
import { ref, onUnmounted, watch, inject } from 'vue';

const props = defineProps({
  isOpen: {
    type: Boolean,
    required: true
  }
});

const emit = defineEmits(['close']);

const closeModal = () => {
  emit('close');
};

const isAddressCopied = ref(false);
const lenis = inject('lenis');

const handleAddressClick = async () => {
  const address = 'Хлібодарське, вул. Покровська, 5а';
  
  try {
    await navigator.clipboard.writeText(address);
    isAddressCopied.value = true;
    
    setTimeout(() => {
      isAddressCopied.value = false;
    }, 2000);
  } catch (err) {
    console.error('Copy Error: ', err);
  }
};

watch(() => props.isOpen, (newVal) => {
  if (newVal) {
    if (lenis && lenis.value) lenis.value.stop();
    
    document.documentElement.style.scrollbarGutter = 'stable';
    document.body.style.overflow = 'hidden'; 
  } else {
    if (lenis && lenis.value) {
      lenis.value.start();
      lenis.value.resize();
    }
    
    document.documentElement.style.scrollbarGutter = '';
    document.body.style.overflow = '';
  }
});

onUnmounted(() => {
  document.documentElement.style.scrollbarGutter = '';
  document.body.style.overflow = '';
});
</script>

<style scoped>
/* ==========================================================================
   OVERLAY BACKDROP
   ========================================================================== */
.modal-overlay {
  position: fixed;
  top: 0px;
  left: 0px;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%; 
  height: 100%; 
  padding: 20px;
  background: rgba(0, 0, 0, 0.7);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  z-index: 9999;
  box-sizing: border-box; 
}

/* ==========================================================================
   MODAL CONTAINER
   ========================================================================== */
.modal-container {
  position: relative;
  top: 0px;
  width: 100%;
  max-width: 580px;
  padding: 48px;
  background: #0F0D0C; 
  border: 1px solid rgba(249, 246, 240, 0.08);
  border-radius: 24px;
  box-shadow: 0 40px 80px rgba(0, 0, 0, 0.5);
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* ==========================================================================
   CLOSE BUTTON (X)
   ========================================================================== */
.btn-close {
  position: absolute;
  top: 24px;
  right: 24px;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 40px;
  height: 40px;
  background: rgba(249, 246, 240, 0.05);
  color: #B8A99A;
  border: none;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-close:hover {
  background: #D49A5E; 
  color: #0F0D0C;
}

.btn-close svg {
  width: 20px;
  height: 20px;
}

/* ==========================================================================
   MODAL TYPOGRAPHY & HEADER
   ========================================================================== */
.modal-header {
  margin-bottom: 40px;
  text-align: center;
}

.subtitle {
  display: block;
  margin-bottom: 12px;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.2em;
  color: #D49A5E; 
}

.title {
  margin: 0 0 16px 0;
  font-size: 2.2rem;
  font-weight: 500;
  letter-spacing: -0.02em;
  color: #F9F6F0;
}

.description {
  margin: 0;
  font-size: 1rem;
  line-height: 1.6;
  color: #B8A99A; 
}

/* ==========================================================================
   CONTACT LIST & CARDS
   ========================================================================== */
.contact-links {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.contact-card {
  position: relative; 
  top: 0px;
  display: flex;
  align-items: center;
  width: 100%;
  gap: 20px;
  padding: 20px 24px;
  background: rgba(249, 246, 240, 0.03);
  border: 1px solid rgba(249, 246, 240, 0.05);
  border-radius: 16px;
  text-decoration: none;
  box-sizing: border-box;
  transition: top 0.4s cubic-bezier(0.16, 1, 0.3, 1), background-color 0.4s ease, border-color 0.4s ease;
  will-change: top, border-color, background-color;
}

.btn-reset {
  appearance: none;
  outline: none;
  font-family: inherit;
  text-align: left;
  cursor: pointer;
}

.contact-card::after {
  content: "";
  position: absolute;
  top: -15px;
  right: -15px;
  bottom: -15px;
  left: -15px;
  z-index: 0;
}

.contact-card:hover {
  background: rgba(249, 246, 240, 0.06);
  border-color: #D49A5E; 
  top: -4px;
}

/* ==========================================================================
   ICONS & TEXT
   ========================================================================== */
.icon-wrapper {
  position: relative; 
  z-index: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-shrink: 0; 
  width: 48px;
  height: 48px;
  color: #F9F6F0;
  background: rgba(249, 246, 240, 0.05);
  border-radius: 50%;
  transition: all 0.4s ease;
}

.contact-card:hover .icon-wrapper {
  color: #0F0D0C;
  background: #D49A5E; 
}

.icon-wrapper svg {
  width: 20px;
  height: 20px;
}

.contact-info {
  position: relative; 
  z-index: 1;
  display: flex;
  flex-direction: column;
  flex-grow: 1;
  gap: 4px;
  overflow: hidden; 
}

.contact-label {
  font-size: 0.85rem;
  font-weight: 500;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: #B8A99A;
  white-space: nowrap;
  transition: color 0.4s ease;
}

.contact-card:hover .contact-label {
  color: #D49A5E;
}

.contact-label.text-success {
  color: #4ade80; 
}

.contact-value {
  font-size: 1.1rem;
  font-weight: 500;
  color: #F9F6F0;
}

.address-value {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

/* ==========================================================================
   VUE TRANSITION ANIMATIONS (NO TRANSFORM/SCALE)
   ========================================================================== */
.modal-fade-enter-active {
  transition: opacity 0.4s ease;
}

.modal-fade-leave-active {
  transition: opacity 0.3s ease;
  pointer-events: none;
}

.modal-fade-enter-active .modal-container {
  transition: top 0.4s cubic-bezier(0.16, 1, 0.3, 1), opacity 0.4s ease;
}

.modal-fade-leave-active .modal-container {
  transition: top 0.3s ease, opacity 0.3s ease;
  pointer-events: none;
}

.modal-fade-enter-from,
.modal-fade-leave-to {
  opacity: 0;
}

.modal-fade-enter-from .modal-container {
  opacity: 0;
  top: 30px;
}

.modal-fade-leave-to .modal-container {
  opacity: 0;
  top: 20px;
}

/* ==========================================================================
   MOBILE RESPONSIVENESS
   ========================================================================== */
@media (max-width: 480px) {
  .modal-container {
    padding: 40px 24px 32px; 
  }
  
  .btn-close {
    top: 16px;
    right: 16px;
    width: 36px;
    height: 36px;
  }

  .title {
    font-size: clamp(24px, 6.5vw, 32px);
    white-space: nowrap; 
  }

  .desktop-only-text {
    display: none;
  }

  .description {
    padding: 0 10px; 
    font-size: 0.95rem;
  }

  .contact-card {
    gap: 12px; 
    padding: 16px 12px;
  }

  .contact-label {
    font-size: clamp(10px, 3.5vw, 12px);
    letter-spacing: 0.05em;
  }

  .contact-value {
    font-size: clamp(13px, 4vw, 16px);
  }
  
  .icon-wrapper {
    width: 36px;
    height: 36px;
  }

  .icon-wrapper svg {
    width: 16px;
    height: 16px;
  }
}
</style>