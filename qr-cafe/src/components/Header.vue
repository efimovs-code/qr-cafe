<template>
  <header
    class="header-section"
    :class="{
      'is-scrolled': isScrolled,
      'is-hidden': isHidden,
    }"
  >
    <div class="header-container">
      <!-- Logo scrolls to the very top -->
      <a href="#top" class="logo" @click="handleNavClick($event, '#top')"> QR CAFE </a>

      <nav class="desktop-nav">
        <!-- Smooth scroll navigation -->
        <a href="#menu" class="nav-link" @click="handleNavClick($event, '#menu')">Меню</a>
        <a href="#expertise" class="nav-link" @click="handleNavClick($event, '#expertise')">Атмосфера</a>
        <a href="#reviews" class="nav-link" @click="handleNavClick($event, '#reviews')">Відгуки</a>
        <a href="#footer" class="nav-link" @click="handleNavClick($event, '#footer')">Контакти</a>
      </nav>

      <div class="desktop-action">
        <div class="btn-wrapper">
          <button class="btn-contact" @click="openContactModal">Забронювати стіл</button>
        </div>
      </div>

      <button
        class="burger-btn"
        :class="{ 'is-active': isMobileMenuOpen }"
        @click="toggleMenu"
        aria-label="Toggle menu"
      >
        <span class="burger-line"></span>
        <span class="burger-line"></span>
      </button>
    </div>

    <!-- Mobile menu -->
    <div class="mobile-menu" :class="{ 'is-open': isMobileMenuOpen }">
      <div class="mobile-menu-content">
        <nav class="mobile-nav">
          <a href="#menu" class="mobile-link" @click="handleNavClick($event, '#menu')">Меню</a>
          <a href="#expertise" class="mobile-link" @click="handleNavClick($event, '#expertise')">Атмосфера</a>
          <a href="#reviews" class="mobile-link" @click="handleNavClick($event, '#reviews')">Відгуки</a>
          <a href="#footer" class="mobile-link" @click="handleNavClick($event, '#footer')">Контакти</a>
        </nav>

        <div class="mobile-footer">
          <span class="mobile-contact">Хлібодарське, Одеська область</span>
          <a href="tel:+380961805976" class="mobile-phone">+380 96 180 5976</a>
        </div>
      </div>
    </div>
  </header>

  <Teleport to="body">
    <ContactModal :isOpen="isModalOpen" @close="isModalOpen = false" />
  </Teleport>
</template>

<script setup>
import { ref, onMounted, onUnmounted, inject } from "vue";
import ContactModal from "./ContactModal.vue";

const isScrolled = ref(false);
const isHidden = ref(false);
const isMobileMenuOpen = ref(false);
const isModalOpen = ref(false);

const lenis = inject('lenis'); 

let lastScrollY = 0;

const handleScroll = () => {
  if (window.innerWidth <= 992) {
    isScrolled.value = false;
    isHidden.value = false;
    lastScrollY = window.scrollY;
    return;
  }

  const currentScrollY = window.scrollY;
  isScrolled.value = currentScrollY > 50;

  if (currentScrollY > lastScrollY && currentScrollY > 200) {
    if (!isMobileMenuOpen.value && !isModalOpen.value) {
      isHidden.value = true;
    }
  } else {
    isHidden.value = false;
  }

  lastScrollY = currentScrollY;
};

const toggleMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value;
  document.body.style.overflow = isMobileMenuOpen.value ? "hidden" : "";
};

const handleNavClick = (e, target) => {
  e.preventDefault(); 

  if (isMobileMenuOpen.value) {
    isMobileMenuOpen.value = false;
    document.body.style.overflow = "";

    if (lenis && lenis.value) {
      lenis.value.resize();
    }

    setTimeout(() => {
      scrollToTarget(target);
    }, 350);
  } else {
    scrollToTarget(target);
  }
};

const scrollToTarget = (target) => {
  const isMobile = window.innerWidth <= 992;

  if (lenis && lenis.value) {
    lenis.value.scrollTo(target === '#top' ? 0 : target, {
      offset: target === '#top' ? 0 : -80,
      duration: isMobile ? 8.5 : 4.0, 
      easing: (t) => 1 - Math.pow(1 - t, 8) 
    });
  } else if (target !== '#top') {
    document.querySelector(target)?.scrollIntoView({ behavior: 'smooth' });
  }
};

const openContactModal = () => {
  isModalOpen.value = true;
};

onMounted(() => {
  window.addEventListener("scroll", handleScroll);
});

onUnmounted(() => {
  window.removeEventListener("scroll", handleScroll);
  document.body.style.overflow = "";
});
</script>

<style scoped>
.header-section {
  position: fixed;
  top: 0px;
  left: 0px;
  width: 100%;
  z-index: 100;
  padding: 32px 0;
  box-sizing: border-box; 
  background-color: transparent;
  transition:
    top 0.4s cubic-bezier(0.16, 1, 0.3, 1),
    background-color 0.4s ease,
    padding 0.4s ease,
    box-shadow 0.4s ease;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.header-section.is-hidden {
  top: -120px;
}

.header-section.is-scrolled {
  padding: 16px 0;
  /* Dark glass effect on scroll */
  background: rgba(15, 13, 12, 0.95);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
}

.header-container {
  max-width: 1400px;
  width: 100%;
  margin: 0 auto;
  padding: 0 40px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  position: relative;
  z-index: 101;
}

.logo {
  font-size: 19.2px;
  font-weight: 700;
  letter-spacing: 2.88px;
  color: #F9F6F0; 
  text-decoration: none;
  text-transform: uppercase;
}

.desktop-nav {
  display: flex;
  gap: 40px;
  position: absolute;
  left: 0;
  right: 0;
  margin: 0 auto;
  width: max-content;
}

.nav-link {
  font-size: 14.4px;
  font-weight: 500;
  color: #B8A99A; 
  text-decoration: none;
  transition: color 0.3s ease;
  position: relative;
}

.nav-link:hover {
  color: #F9F6F0; 
}

.nav-link::after {
  content: "";
  position: absolute;
  bottom: -4px;
  left: 0px;
  width: 0%;
  height: 2px;
  background-color: #D49A5E; 
  transition: width 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}

.nav-link:hover::after {
  width: 100%;
}

.btn-wrapper {
  display: inline-flex;
  padding-top: 4px;
  margin-top: -4px;
}

.btn-contact {
  position: relative;
  top: 0px;
  left: 0px;
  padding: 12px 32px; 
  background-color: transparent;
  color: #F9F6F0;
  border: 1px solid rgba(249, 246, 240, 0.2);
  border-radius: 100px; 
  font-size: 13.6px;
  font-weight: 600; 
  text-transform: uppercase;
  letter-spacing: 0.68px;
  cursor: pointer;
  transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1);
  will-change: top, background-color, box-shadow, border-color;
}

.btn-wrapper:hover .btn-contact {
  background-color: #D49A5E; /* Accent fill on hover */
  color: #0F0D0C;
  border-color: #D49A5E;
  top: -4px;
  box-shadow: 0 8px 20px rgba(212, 154, 94, 0.2);
}

.burger-btn {
  display: none;
  position: relative;
  width: 40px; 
  height: 40px;
  background: transparent;
  border: none;
  cursor: pointer;
  z-index: 102;
}

.burger-line {
  position: absolute;
  left: 0px;
  right: 0px;
  margin: 0 auto;
  width: 24px;
  height: 1px;
  background-color: #F9F6F0; 
  transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}

.burger-line:first-child { top: 15px; }
.burger-line:last-child { bottom: 15px; }

.burger-btn.is-active .burger-line:first-child {
  top: 19px;
  rotate: 45deg;
}

.burger-btn.is-active .burger-line:last-child {
  bottom: 20px;
  rotate: -45deg;
}

.mobile-menu {
  position: fixed;
  top: 0px;
  left: 0px;
  width: 100%;
  height: 100vh;
  background-color: #0F0D0C;
  z-index: 100;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 0 40px;
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.3s ease, visibility 0.3s ease;
}

.mobile-menu.is-open {
  opacity: 1;
  visibility: visible;
}

.mobile-menu-content {
  display: flex;
  flex-direction: column;
  width: 100%;
  gap: 60px;
}

.mobile-nav {
  display: flex;
  flex-direction: column;
  width: 100%;
}

.mobile-link {
  font-size: 18.4px;
  font-weight: 500;
  color: #F9F6F0;
  text-decoration: none;
  text-transform: uppercase;
  letter-spacing: 2.76px;
  padding: 24px 0; 
  border-bottom: 1px solid rgba(249, 246, 240, 0.1); 
  display: block;
  width: 100%;
  text-align: left;
  transition: color 0.3s ease;
}

.mobile-link:first-child {
  border-top: 1px solid rgba(249, 246, 240, 0.1);
}

.mobile-link:hover {
  color: #D49A5E;
}

.mobile-footer {
  display: flex;
  flex-direction: column;
  gap: 8px;
  padding-top: 16px;
}

.mobile-contact {
  font-size: 13.6px;
  text-transform: uppercase;
  letter-spacing: 1.36px;
  color: #B8A99A;
}

.mobile-phone {
  font-size: 20px;
  color: #D49A5E;
  text-decoration: none;
  font-weight: 500;
}

@media (max-width: 992px) {
  .desktop-nav,
  .desktop-action { display: none; }
  .burger-btn { display: block; }
  
  .header-section,
  .header-section.is-scrolled,
  .header-section.is-hidden {
    position: fixed !important;
    top: 0px !important;
    padding: 20px 0 !important;
    background: #0F0D0C !important; 
    backdrop-filter: none !important;
    -webkit-backdrop-filter: none !important;
    box-shadow: none !important;
    transition: none !important;
  }
}

@media (max-width: 768px) {
  .header-container, .mobile-menu { padding: 0 24px; }
}
</style>