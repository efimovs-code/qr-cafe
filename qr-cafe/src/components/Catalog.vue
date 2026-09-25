<template>
  <section id="menu" class="catalog-section">
    
    <!-- ==========================================================================
         SECTION HEADER
         ========================================================================== -->
    <div class="catalog-container header-container">
      <div class="section-header">
        <span class="subtitle animate-on-scroll fade-up">Наше Меню</span>
        <h2 class="title animate-on-scroll fade-up delay-100">
          Смак, який об'єднує
        </h2>
      </div>
    </div>

    <!-- ==========================================================================
         INTERACTIVE MENU (TABS AND CONTENT)
         ========================================================================== -->
    <div class="catalog-container menu-interactive-wrapper animate-on-scroll fade-up delay-200">
      
      <!-- Navigation Menu (Tabs) -->
      <div class="menu-tabs-container">
        
        <div class="menu-tabs" ref="tabsMenu">
          <button 
            v-for="category in menuCategories" 
            :key="category.id"
            class="tab-btn"
            :class="{ 'active': activeCategory === category.id }"
            @click="selectCategory(category.id, $event)"
          >
            {{ category.name }}
          </button>
        </div>
        
      </div>

      <!-- Selected Category Content -->
      <div class="menu-content-area">
        
        <!-- Left column: Dynamic category photo -->
        <div class="menu-image-col">
          <Transition name="fade-img" mode="out-in">
            <img 
              :key="currentCategoryData.id"
              :src="currentCategoryData.image" 
              :alt="currentCategoryData.name"
              class="category-image"
              loading="lazy"
            />
          </Transition>
          <div class="image-overlay"></div>
        </div>

        <!-- Right column: Dish list -->
        <div class="menu-list-col">
          <Transition name="fade-list" mode="out-in">
            <div :key="currentCategoryData.id" class="menu-items-wrapper">
              <h3 class="mobile-category-title">{{ currentCategoryData.name }}</h3>
              
              <div 
                v-for="(item, index) in currentCategoryData.items" 
                :key="index" 
                class="menu-item-row"
              >
                <div class="item-info">
                  <span class="item-name">{{ item.name }}</span>
                  <span v-if="item.desc" class="item-desc">{{ item.desc }}</span>
                </div>
                
                <div class="item-dots"></div>
                
                <div class="item-meta">
                  <span v-if="item.weight" class="item-weight">{{ item.weight }}</span>
                  <span class="item-price">{{ item.price }} ₴</span>
                </div>
              </div>

            </div>
          </Transition>
        </div>

      </div>
    </div>

    <!-- ==========================================================================
         FINAL CTA BLOCK
         ========================================================================== -->
    <div class="project-row cta-row">
      <div class="catalog-container">
        <div class="catalog-cta">
          <div class="cta-container">
            <div class="btn-wrapper animate-on-scroll fade-up">
              <button class="btn-cta" @click="openModal">
                <span>Забронювати стіл</span>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <Teleport to="body">
      <ContactModal :isOpen="isModalOpen" @close="isModalOpen = false" />
    </Teleport>
  </section>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue';
import ContactModal from './ContactModal.vue'; 

const isModalOpen = ref(false);

const openModal = () => {
  isModalOpen.value = true;
};

// ============================================================================
// SCROLL LOGIC AND ACTIVE CATEGORY
// ============================================================================
const tabsMenu = ref(null);

const selectCategory = (id, event) => {
  activeCategory.value = id;
  
  // UX Magic: Smoothly center the button on the screen upon click
  if (event && event.target) {
    event.target.scrollIntoView({
      behavior: 'smooth',
      inline: 'center',
      block: 'nearest'
    });
  }
};

// ============================================================================
// FULL MENU DATABASE
// ============================================================================
const menuCategories = [
  {
    id: 'breakfast',
    name: 'Сніданки',
    image: 'https://images.unsplash.com/photo-1533089860892-a7c6f0a88666?q=80&w=1000&auto=format&fit=crop',
    items: [
      { name: 'Яєчня з беконом', desc: 'Подається з тостами', weight: '150 г', price: '75' },
      { name: 'Англійський сніданок з 3-х яєць', desc: 'Подається з тостами та мисливськими ковбасками', weight: '280 г', price: '95' },
      { name: 'Омлет з овочами', desc: '', weight: '200 г', price: '75' },
      { name: 'Сирники зі сметаною', desc: '', weight: '150 г', price: '100' },
      { name: 'Млинці з м\'ясом', desc: 'Подаються зі сметаною', weight: '250 г', price: '100' },
    ]
  },
  {
    id: 'salads',
    name: 'Салати',
    image: 'https://images.unsplash.com/photo-1512621776951-a57141f2eefd?q=80&w=1000&auto=format&fit=crop',
    items: [
      { name: 'Салат з капустою', desc: 'Капуста, огірок, зелень, олія', weight: '200 г', price: '60' },
      { name: 'Салат зі свіжих овочів', desc: 'Помідор, огірок, цибуля, зелень', weight: '200 г', price: '95' },
      { name: 'Салат Грецький', desc: 'Помідор, огірок, болгарський перець, фета, цибуля, маслини', weight: '200 г', price: '175' },
      { name: 'Цезар з куркою', desc: 'Куряче філе, мікс салату, томати, пармезан, соус, сухарики, яйце', weight: '250 г', price: '215' },
    ]
  },
  {
    id: 'soups',
    name: 'Перші страви',
    image: 'https://images.unsplash.com/photo-1547592180-85f173990554?q=80&w=1000&auto=format&fit=crop',
    items: [
      { name: 'Український борщ', desc: 'Подається зі сметаною', weight: '250 г', price: '85' },
      { name: 'Солянка', desc: '', weight: '250 г', price: '95' },
      { name: 'Гороховий суп', desc: '', weight: '250 г', price: '75' },
      { name: 'Курячий суп з локшиною', desc: '', weight: '250 г', price: '75' },
      { name: 'Суп з фрикадельками', desc: '', weight: '250 г', price: '75' },
    ]
  },
  {
    id: 'hot-dishes',
    name: 'Гарячі страви',
    image: 'https://images.unsplash.com/photo-1600891964092-4316c288032e?q=80&w=1000&auto=format&fit=crop',
    items: [
      { name: 'Печеня по-домашньому', desc: 'Картопля, м\'ясо, цибуля, морква, часник, зелень', weight: '400 г', price: '140' },
      { name: 'Голубці зі сметаною', desc: '', weight: '300 г', price: '100' },
      { name: 'Курча табака', desc: '', weight: '1/2 курчати', price: '260' },
      { name: 'Курячі крильця', desc: '', weight: '200 г', price: '155' },
      { name: 'Відбивна зі свинини', desc: '', weight: '150 г', price: '120' },
      { name: 'Куряча відбивна', desc: '', weight: '150 г', price: '120' },
      { name: 'Купати', desc: '', weight: '250 г', price: '150' },
    ]
  },
  {
    id: 'sides',
    name: 'Гарніри',
    image: 'https://images.unsplash.com/photo-1590165482129-1b8b27698780?q=80&w=1000&auto=format&fit=crop',
    items: [
      { name: 'Картопля фрі', desc: 'Подається з кетчупом', weight: '150 г', price: '85' },
      { name: 'Картопля по-домашньому', desc: 'З грибами та цибулею', weight: '250 г', price: '90' },
      { name: 'Картопля по-селянськи', desc: 'Подається з часниковим соусом', weight: '200 г', price: '90' },
      { name: 'Пюре', desc: '', weight: '200 г', price: '60' },
      { name: 'Локшина', desc: '', weight: '200 г', price: '50' },
      { name: 'Пельмені', desc: '', weight: '250 г', price: '95' },
    ]
  },
  {
    id: 'grill',
    name: 'Мангал',
    image: 'https://images.unsplash.com/photo-1555939594-58d7cb561ad1?q=80&w=1000&auto=format&fit=crop',
    items: [
      { name: 'Люля-кебаб курячий', desc: '', weight: '200 г', price: '220' },
      { name: 'Люля-кебаб яловичий', desc: '', weight: '200 г', price: '260' },
      { name: 'Люля-кебаб з баранини', desc: '', weight: '200 г', price: '280' },
      { name: 'Скумбрія', desc: '', weight: '1 шт', price: '250' },
      { name: 'Шашлик з ошийка', desc: '', weight: '250 г', price: '250' },
      { name: 'Крильце куряче', desc: '', weight: '200 г', price: '200' },
      { name: 'Стегно куряче', desc: '', weight: '200 г', price: '190' },
      { name: 'Овочі гриль', desc: '', weight: '200 г', price: '190' },
      { name: 'Гриби гриль', desc: '', weight: '200 г', price: '140' },
    ]
  },
  {
    id: 'pizza',
    name: 'Піца / Випічка',
    image: 'https://images.unsplash.com/photo-1565299624946-b28f40a0ae38?q=80&w=1000&auto=format&fit=crop',
    items: [
      { name: 'Мексиканська (35 см)', desc: 'Соус чилі, моцарела, куряче філе, кукурудза, болгарський перець, помідор', weight: '', price: '245' },
      { name: 'Сільська (35 см)', desc: 'Соус білий, моцарела, балик, мисливські ковбаски, маринований огірок, цибуля, томати', weight: '', price: '245' },
      { name: 'Цезар (35 см)', desc: 'Білий соус, моцарела, куряче філе, яйце, томати, мікс салату', weight: '', price: '245' },
      { name: 'Піца QR (35 см)', desc: 'Соус пелаті, моцарела, печериці, балик, мисливські ковбаски, бекон, болгарський перець, томати, оливки', weight: '', price: '245' },
      { name: '4 м\'яса (35 см)', desc: 'Соус пелаті, моцарела, мисливські ковбаски, салямі, шинка, куряче філе, бекон, маслини, цибуля', weight: '', price: '245' },
      { name: 'Хачапурі по-аджарськи', desc: '', weight: '', price: '200' },
      { name: 'Хачапурі по-мегрельськи', desc: '', weight: '', price: '220' },
    ]
  },
  {
    id: 'drinks',
    name: 'Напої',
    image: 'https://images.unsplash.com/photo-1622483767028-3f66f32aef97?q=80&w=1000&auto=format&fit=crop',
    items: [
      { name: 'PEPSI скло', desc: '', weight: '0.3 л', price: '35' },
      { name: 'PEPSI бляшанка', desc: '', weight: '0.33 л', price: '35' },
      { name: 'PEPSI', desc: '', weight: '0.5 л', price: '35' },
      { name: 'PEPSI', desc: '', weight: '1 л', price: '50' },
      { name: 'Сік в асортименті', desc: '', weight: '0.2 л', price: '20' },
      { name: 'Компот', desc: '', weight: '0.2 л', price: '20' },
    ]
  },
  {
    id: 'alcohol',
    name: 'Бар',
    image: 'https://images.unsplash.com/photo-1514362545857-3bc16c4c7d1b?q=80&w=1000&auto=format&fit=crop',
    items: [
      { name: 'Джин Gordon\'s / Finsbury', desc: '', weight: '50 г', price: '60 / 50' },
      { name: 'Ром Oakheart / Bacardi', desc: '', weight: '50 г', price: '60' },
      { name: 'Ром Captain Morgan', desc: '', weight: '50 г', price: '70' },
      { name: 'Вермут Campari / Aperol', desc: '', weight: '50 г', price: '100' },
      { name: 'Вермут Martini / Cinzano', desc: '', weight: '50 г', price: '40' },
      { name: 'Вино в асортименті', desc: '', weight: '1 пл', price: '250' },
      { name: 'Шампанське «Одеса»', desc: '', weight: '1 пл', price: '300' },
      { name: 'Горілка Хортиця Платинум / Преміум', desc: '', weight: '50 г', price: '25 / 30' },
      { name: 'Горілка Nemiroff LEX / Особлива', desc: '', weight: '50 г', price: '50 / 20' },
      { name: 'Горілка Green Day / Хлібний Дар', desc: '', weight: '50 г', price: '20' },
    ]
  }
];

const activeCategory = ref(menuCategories[0].id);

const currentCategoryData = computed(() => {
  return menuCategories.find(cat => cat.id === activeCategory.value) || menuCategories[0];
});

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
.catalog-section {
  width: 100%;
  padding-top: 140px; 
  background-color: #0F0D0C; 
  color: #F9F6F0;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.catalog-container {
  max-width: 1400px; 
  width: 100%;
  margin: 0 auto;
  padding: 0 40px;
}

.header-container {
  max-width: 1200px; 
}

/* ==========================================================================
   HEADER
   ========================================================================== */
.section-header {
  margin-bottom: 50px; 
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
  font-weight: 600;
  line-height: 1.15; 
  letter-spacing: -1.12px;
  color: #F9F6F0; 
}

/* ==========================================================================
   MENU TABS (NAVIGATION) - WITH ELEGANT SCROLLBAR
   ========================================================================== */
.menu-tabs-container {
  position: relative; 
  display: flex;
  justify-content: center;
  width: 100%;
  margin-bottom: 40px;
}

.menu-tabs {
  display: flex;
  gap: 12px;
  padding: 8px 8px 12px 8px;
  background: rgba(25, 22, 20, 0.8);
  border: 1px solid rgba(212, 154, 94, 0.15);
  border-radius: 40px;
  backdrop-filter: blur(24px);
  overflow-x: auto;
  
  scrollbar-width: thin;
  scrollbar-color: rgba(212, 154, 94, 0.4) transparent;
}

/* --- PREMIUM SCROLLBAR FOR Chrome, Safari, Edge, iOS --- */
.menu-tabs::-webkit-scrollbar {
  display: block;
  height: 3px; 
}

.menu-tabs::-webkit-scrollbar-track {
  background: transparent; 
  margin: 0 30px; /* Offsets from the edges of the capsule so the line does not hit the walls */
  border-radius: 10px;
}

.menu-tabs::-webkit-scrollbar-thumb {
  background: rgba(212, 154, 94, 0.4); /* Semi-transparent gold */
  border-radius: 10px;
}

.menu-tabs::-webkit-scrollbar-thumb:hover,
.menu-tabs::-webkit-scrollbar-thumb:active {
  background: rgba(212, 154, 94, 0.8); 
}

.tab-btn {
  padding: 14px 28px;
  font-size: 15px;
  font-weight: 500;
  color: #B8A99A;
  background: transparent;
  border: none;
  border-radius: 100px;
  white-space: nowrap;
  cursor: pointer;
  transition: all 0.3s ease;
}

.tab-btn:hover {
  color: #F9F6F0;
}

.menu-tabs .tab-btn.active {
  color: #0F0D0C !important;
  background-color: #D49A5E !important;
  box-shadow: 0 4px 15px rgba(212, 154, 94, 0.4) !important;
}

/* ==========================================================================
   MENU CONTENT AREA
   ========================================================================== */
.menu-interactive-wrapper {
  margin-bottom: 80px;
}

.menu-content-area {
  display: flex;
  gap: 40px;
  background: rgba(25, 22, 20, 0.6);
  border: 1px solid rgba(212, 154, 94, 0.1);
  border-radius: 32px;
  padding: 24px;
  min-height: 600px;
}

.menu-image-col {
  flex: 0 0 40%;
  position: relative;
  border-radius: 24px;
  overflow: hidden;
  background: #111;
}

.category-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  position: absolute;
  top: 0px;
  left: 0px;
}

.image-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(to right, rgba(15, 13, 12, 0.1), rgba(15, 13, 12, 0.6));
}

.menu-list-col {
  flex: 1;
  padding: 20px 40px 20px 20px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.menu-items-wrapper {
  position: relative;
  left: 0px;
  display: flex;
  flex-direction: column;
  gap: 24px;
  max-height: 600px;
  overflow-y: auto;
  padding-right: 16px;
  scrollbar-width: thin;
  scrollbar-color: #D49A5E transparent;
}

.menu-items-wrapper::-webkit-scrollbar { width: 4px; }
.menu-items-wrapper::-webkit-scrollbar-thumb { background-color: #D49A5E; border-radius: 10px; }
.mobile-category-title { display: none; }

.menu-item-row {
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  width: 100%;
}

.item-info {
  display: flex;
  flex-direction: column;
  gap: 4px;
  max-width: 65%;
}

.item-name { font-size: 20px; font-weight: 500; color: #F9F6F0; }
.item-desc { font-size: 13.6px; line-height: 1.4; color: #B8A99A; }
.item-dots { flex: 1; border-bottom: 1px dashed rgba(184, 169, 154, 0.3); margin: 0 16px 6px; }
.item-meta { display: flex; align-items: center; gap: 16px; white-space: nowrap; }
.item-weight { font-size: 14px; color: #B8A99A; }
.item-price { font-size: 20px; font-weight: 600; color: #D49A5E; }

/* ==========================================================================
   VUE ANIMATIONS (TRANSITIONS)
   ========================================================================== */
.fade-img-enter-active, .fade-img-leave-active { 
  transition: opacity 0.5s ease, width 0.5s ease, height 0.5s ease, top 0.5s ease, left 0.5s ease; 
}
.fade-list-enter-active, .fade-list-leave-active { 
  transition: opacity 0.5s ease, left 0.5s ease; 
}
.fade-img-enter-from, .fade-img-leave-to { 
  opacity: 0; 
  top: -2.5%; 
  left: -2.5%; 
  width: 105%; 
  height: 105%; 
}
.fade-list-enter-from { opacity: 0; left: 20px; }
.fade-list-leave-to { opacity: 0; left: -20px; }

/* ==========================================================================
   CALL TO ACTION (CTA) AND SCROLL ANIMATIONS
   ========================================================================== */
.cta-row { padding: 40px 0 140px; }
.catalog-cta { display: flex; justify-content: center; align-items: center; width: 100%; text-align: center; }
.cta-container { display: flex; flex-direction: column; align-items: center; max-width: 800px; padding: 0 40px; }
.btn-wrapper { display: inline-flex; margin-top: -4px; padding-top: 4px; }

.btn-cta {
  position: relative;
  top: 0px;
  display: inline-flex; align-items: center; justify-content: center; padding: 20px 56px; 
  font-size: 13px; font-weight: 700; text-transform: uppercase; letter-spacing: 2px;
  color: #0F0D0C; background-color: #D49A5E; border: none; border-radius: 100px; 
  cursor: pointer; transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1);
  will-change: top, background-color, box-shadow;
}
.btn-cta:hover { background-color: #E2AD75; top: -4px; box-shadow: 0 15px 30px rgba(212, 154, 94, 0.25); }

.animate-on-scroll { position: relative; opacity: 0; transition: opacity 1.2s cubic-bezier(0.16, 1, 0.3, 1), top 1.2s cubic-bezier(0.16, 1, 0.3, 1) !important; will-change: top, opacity; }
.fade-up { top: 40px; }
.animate-on-scroll.is-revealed { opacity: 1; top: 0px; }
.delay-100 { transition-delay: 0.1s !important; }
.delay-200 { transition-delay: 0.2s !important; }

/* ==========================================================================
   RESPONSIVE DESIGN (MOBILE VERSION)
   ========================================================================== */
@media (max-width: 1024px) {
  .menu-content-area { flex-direction: column; padding: 16px; gap: 24px; min-height: auto; }
  .menu-image-col { width: 100%; height: 300px; flex: none; }
  .image-overlay { background: linear-gradient(to bottom, rgba(15, 13, 12, 0.1), rgba(15, 13, 12, 0.8)); }
  .menu-list-col { padding: 0 8px 24px; }
  
  .menu-items-wrapper { max-height: none; overflow-y: visible; padding-right: 0; }
  .menu-items-wrapper::-webkit-scrollbar { display: none; }
}

@media (max-width: 768px) {
  .catalog-section { padding-top: 100px; }
  .section-header { margin-bottom: 30px; }
  .title { font-size: 38px; }
  
  .menu-tabs-container {
    justify-content: flex-start;
  }

  .menu-tabs {
    width: 100%; 
  }

  .tab-btn {
    padding: 12px 20px;
    font-size: 14px;
  }

  .mobile-category-title {
    display: block; margin: 0 0 24px 0; font-size: 26px; color: #D49A5E; text-align: left; 
  }

  .menu-item-row {
    flex-direction: row; align-items: flex-start; justify-content: space-between; gap: 12px;
    border-bottom: 1px solid rgba(184, 169, 154, 0.15); padding-bottom: 16px; margin-bottom: 16px; 
  }

  .item-dots { display: none; }
  .item-info { max-width: 65%; gap: 6px; }
  .item-name { font-size: 17px; line-height: 1.3; }
  .item-desc { font-size: 13px; }
  
  .item-meta { width: auto; flex-direction: column-reverse; justify-content: flex-start; align-items: flex-end; gap: 6px; }
  .item-price { font-size: 17px; }
  .item-weight { font-size: 13px; }
  .cta-row { padding: 40px 0 80px; }
  .btn-wrapper, .btn-cta { width: 100%; }
}

@media (max-width: 480px) {
  .title { font-size: 28px; } 
  .menu-image-col { height: 200px; }
  .tab-btn { padding: 10px 16px; font-size: 13px; }
  .item-name, .item-price { font-size: 15px; }
  .item-desc, .item-weight { font-size: 12px; }
}
</style>