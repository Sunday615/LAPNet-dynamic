<template>
  <div class="slideshow-container">
    <!-- IMAGES -->
    <img
      v-for="(img, i) in images"
      :key="i"
      :src="img"
      class="slide-img"
      :ref="(el) => (imageRefs[i] = el)"
      alt=""
      draggable="false"
    />

    <!-- Modern arrows -->
    <button class="nav nav--prev" type="button" @click="prevImage" aria-label="Previous slide">
      <svg viewBox="0 0 24 24" class="icon" aria-hidden="true">
        <path d="M15 18l-6-6 6-6" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
      </svg>
    </button>

    <button class="nav nav--next" type="button" @click="nextImage" aria-label="Next slide">
      <svg viewBox="0 0 24 24" class="icon" aria-hidden="true">
        <path d="M9 6l6 6-6 6" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
      </svg>
    </button>

    <!-- Side dots pagination -->
    <div class="pagination-bottom" aria-label="Slide pagination">
      <button
        v-for="(_, i) in images"
        :key="i"
        class="dot"
        :class="{ active: i === currentIndex }"
        type="button"
        @click="goTo(i)"
        :aria-label="`Go to slide ${i + 1}`"
        :aria-current="i === currentIndex ? 'true' : 'false'"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, onBeforeUnmount, onMounted } from "vue";
import gsap from "gsap";

const images = ref([
  "/swiper/1.png",
  "/swiper/2.png",
  "/swiper/3.png",
  "/swiper/4.png",
  "/swiper/5.png",
]);

const imageRefs = []; // DOM refs
const currentIndex = ref(0);
let intervalID = null;

const fadeTo = (nextIndex) => {
  if (nextIndex === currentIndex.value) return;

  const current = imageRefs[currentIndex.value];
  const next = imageRefs[nextIndex];
  if (!current || !next) return;

  // ensure next is above
  gsap.set(next, { zIndex: 2 });
  gsap.set(current, { zIndex: 1 });

  gsap.to(current, { opacity: 0, duration: 0.9, ease: "power2.out" });
  gsap.to(next, { opacity: 1, duration: 0.9, ease: "power2.out" });

  currentIndex.value = nextIndex;
};

const startAutoPlay = () => {
  clearInterval(intervalID);
  intervalID = setInterval(() => {
    const next = (currentIndex.value + 1) % images.value.length;
    fadeTo(next);
  }, 5000);
};

const goTo = (i) => {
  startAutoPlay();
  fadeTo(i);
};

const nextImage = () => {
  startAutoPlay();
  const next = (currentIndex.value + 1) % images.value.length;
  fadeTo(next);
};

const prevImage = () => {
  startAutoPlay();
  const prev = (currentIndex.value - 1 + images.value.length) % images.value.length;
  fadeTo(prev);
};

onMounted(() => {
  // initial setup
  gsap.set(imageRefs, { opacity: 0, zIndex: 0 });
  gsap.set(imageRefs[0], { opacity: 1, zIndex: 1 });

  startAutoPlay();
});

onBeforeUnmount(() => {
  clearInterval(intervalID);
});
</script>

<style scoped>
/* Dark blue modern gradient */
.slideshow-container {
  width: 100%;
  height: 60vh;
  position: relative;
  overflow: hidden;


  background:
    radial-gradient(900px 600px at 70% 15%, rgba(96,165,250,.22), transparent 55%),
    radial-gradient(800px 520px at 30% 85%, rgba(167,139,250,.18), transparent 55%),
    linear-gradient(180deg, #03081a, #01030f);
}

.slide-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: top;
  position: absolute;
  top: 0;
  left: 0;
  opacity: 0;
  user-select: none;
  -webkit-user-drag: none;
}

/* Modern arrows */
.nav {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 46px;
  height: 46px;
  border-radius: 999px;
  border: 1px solid rgba(255,255,255,.18);
  background: rgba(255,255,255,.10);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  color: rgba(234,241,255,.95);
  display: grid;
  place-items: center;
  cursor: pointer;
  box-shadow: 0 18px 50px rgba(0,0,0,.45);
  transition: transform .18s ease, background .18s ease, opacity .18s ease;
  z-index: 5;
}

.nav:hover {
  background: rgba(255,255,255,.14);
  transform: translateY(-50%) scale(1.04);
}

.nav:active {
  transform: translateY(-50%) scale(.98);
}

.nav:focus-visible {
  outline: 2px solid rgba(96,165,250,.65);
  outline-offset: 2px;
}

.nav--prev { left: 14px; }
.nav--next { right: 14px; }

.icon { width: 22px; height: 22px; }

/* Bottom center dots pagination */
.pagination-bottom {
  position: absolute;
  left: 50%;
  bottom: 14px;
  transform: translateX(-50%);
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 10px;
  padding: 10px 12px;
  border-radius: 999px;
  background: rgba(255,255,255,.06);
  border: 1px solid rgba(255,255,255,.12);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  z-index: 6;
}

.dot {
  width: 10px;
  height: 10px;
  border-radius: 999px;
  border: 1px solid rgba(255,255,255,.22);
  background: rgba(234,241,255,.28);
  cursor: pointer;
  transition: transform .18s ease, background .18s ease, box-shadow .18s ease;
}

.dot:hover {
  transform: scale(1.15);
  background: rgba(234,241,255,.42);
}

.dot.active {
  background: linear-gradient(135deg, rgba(96,165,250,.95), rgba(167,139,250,.95));
  box-shadow:
    0 0 0 6px rgba(96,165,250,.14),
    0 14px 40px rgba(96,165,250,.18);
  border-color: rgba(255,255,255,.35);
}

/* Make it mobile friendly */
@media (max-width: 640px) {
  .pagination-bottom {
    bottom: 10px;
    gap: 8px;
    padding: 8px 10px;
  }
.nav { width: 42px; height: 42px; }
}
</style>
