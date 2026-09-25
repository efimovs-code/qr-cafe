<template>
  <a
    href="https://efimovs.agency"
    target="_blank"
    rel="noopener noreferrer"
    class="elite-watermark"
    :class="`theme-${theme}`"
    aria-label="Created by Efimovs Agency"
  >
    <div class="watermark-text">
      <span class="watermark-top" v-once>Created by</span>
      <span class="watermark-bottom" v-once>Efimovs Agency</span>
    </div>
    <img
      src="../assets/transparent.png"
      alt="Efimovs Agency"
      class="watermark-logo"
    />
  </a>
</template>

<script setup>
defineProps({
  theme: {
    type: String,
    default: "dark", 
    validator: (value) => ["light", "dark"].includes(value),
  },
});
</script>

<style scoped>
/* ==========================================================================
   BASE WATERMARK CONTAINER
   ========================================================================== */
.elite-watermark {
  display: inline-flex;
  align-items: center;
  gap: 16px;
  text-decoration: none;
  cursor: pointer;
  transition: opacity 0.3s ease;
}

@media (hover: hover) and (pointer: fine) {
  .elite-watermark:hover {
    opacity: 0.7;
  }
}

/* ==========================================================================
   TEXT TYPOGRAPHY & LAYOUT
   ========================================================================== */
.watermark-text {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 2px;
  font-family:
    "Montserrat",
    "Inter",
    -apple-system,
    BlinkMacSystemFont,
    "Segoe UI",
    Roboto,
    sans-serif;
}

.watermark-top {
  font-size: 9px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 3.15px;
  margin-right: -3.15px;
  transition: color 0.3s ease;
}

.watermark-bottom {
  margin: 0;
  font-size: 18px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.9px;
  transition: color 0.3s ease;
}

/* ==========================================================================
   BRANDING ASSET
   ========================================================================== */
.watermark-logo {
  width: auto;
  height: 46px;
  object-fit: contain;
  image-rendering: high-quality;
  transition: filter 0.3s ease;
}

/* ==========================================================================
   THEMES (COLOR LOGIC)
   ========================================================================== */
/* THEME: DARK (For placement on light, white, or gray backgrounds) */
.theme-dark .watermark-top {
  color: rgba(5, 5, 5, 0.65); 
}
.theme-dark .watermark-bottom {
  color: #050505;
}
.theme-dark .watermark-logo {
  filter: invert(1) drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.05));
}

/* THEME: LIGHT (For placement on black or dark backgrounds) */
.theme-light .watermark-top {
  color: rgba(255, 255, 255, 0.8);
}
.theme-light .watermark-bottom {
  color: #ffffff;
}
.theme-light .watermark-logo {
  filter: none;
}

/* ==========================================================================
   MOBILE RESPONSIVENESS
   ========================================================================== */
@media (max-width: 767px) {
  .elite-watermark {
    gap: 12px;
  }
  .watermark-top {
    font-size: 8px;
  }
  .watermark-bottom {
    font-size: 15px;
  }
  .watermark-logo {
    height: 40px;
  }
}
</style>