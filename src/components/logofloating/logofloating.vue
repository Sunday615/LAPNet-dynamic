<template>
  <div class="logo-container" ref="containerRef">
    <div
      v-for="(logo, index) in logos"
      :key="index"
      class="logo"
      :style="{ left: logo.left + 'px' }"
      ref="logoRefs"
    >
      <img :src="logo.src" :alt="logo.alt" />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import gsap from "gsap";

const logoRefs = ref([]);
const containerRef = ref(null);

const logos = [
  { src: "/logos/logo1.png", alt: "BCEL Mobilebanking", left: 40 },
  { src: "/logos/logo3.png", alt: "APB Mobilebanking", left: 260 },
  { src: "/logos/logo2.png", alt: "LDB Mobilebanking", left: 140 },
  { src: "/logos/logo4.png", alt: "JDB Mobilebanking", left: 360 },
  { src: "/logos/logo5.png", alt: "MARU Mobilebanking", left: 480 },
  { src: "/logos/logo6.png", alt: "LVB Mobilebanking", left: 500 },

  { src: "/logos/logo7.png", alt: "ICBC Mobilebanking", left: 40 },
  { src: "/logos/logo8.png", alt: "BOC Mobilebanking", left: 140 },
  { src: "/logos/logo9.png", alt: "VTB Mobilebanking", left: 260 },
  { src: "/logos/logo10.png", alt: "IB Mobilebanking", left: 360 },
  { src: "/logos/logo11.png", alt: "BIC Mobilebanking", left: 480 },
  { src: "/logos/logo12.png", alt: "BFL Mobilebanking", left: 500 },

  { src: "/logos/logo13.png", alt: "SACOM Mobilebanking", left: 40 },
  { src: "/logos/logo14.png", alt: "PSVB Mobilebanking", left: 140 },
  { src: "/logos/logo15.png", alt: "MB Mobilebanking", left: 260 },
  { src: "/logos/logo16.png", alt: "STB Mobilebanking", left: 360 },
  { src: "/logos/logo17.png", alt: "KBANK Mobilebanking", left: 480 },
  { src: "/logos/logo18.png", alt: "", left: 500 },

   { src: "/logos/logo19.png", alt: "Mmoney", left: 40 },
  { src: "/logos/logo20.png", alt: "Umoney", left: 140 },
];

onMounted(() => {
  const items = logoRefs.value.filter(Boolean);

  const containerHeight = 600;
  const logoHeight = 110;
  const total = items.length;


  const spacing = logoHeight * 1.9;      
  const distance = spacing * total;

  const startY = containerHeight + logoHeight; 
  const endY = startY - distance;              

  const travelDuration = 30;               
  const slot = travelDuration / total;         

  items.forEach((el, i) => {
    const delay = i * slot;

    
    gsap.set(el, {
      y: startY,
      opacity: 1,
      x: 0,
    });

  
    gsap.to(el, {
      y: endY,
      duration: travelDuration,
      ease: "none",
      delay,
      repeat: -1,
      repeatDelay: 0,
    });


    gsap.to(el, {
      x: i % 2 === 0 ? "+=70" : "-=70", 
      duration: 4,
      yoyo: true,
      repeat: -1,
      ease: "sine.inOut",
      delay: delay * 0.3, 
    });
  });

  const container = containerRef.value;

  if (container) {
    const tl = gsap.timeline({
      repeat: -1,
      yoyo: true,
      ease: "sine.inOut",
    });

    tl.to(container, {
      duration: 6,
      y: -20,
      x: 10,
      rotation: 1.5,
    })
      .to(
        container,
        {
          duration: 6,
          y: -10,
          x: -15,
          rotation: -1.5,
        },
        "+=0"
      )
      .to(
        container,
        {
          duration: 6,
          y: 5,
          x: 5,
          rotation: 0,
        },
        "+=0"
      );
  }
});
</script>

<style scoped>
.logo-container {
  position: relative;
  width: 100%;
  height: 50vh;
  overflow: hidden;
  margin: 0 auto;
}

.logo {
  position: absolute;
  width: 150px;
  height: 150px;
  border-radius: 50%;
  background: #ffffff;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.18);
}

.logo img {
  width: 70%;
  height: 70%;
  object-fit: contain;
}
@media (max-width: 980px) {

  .logo {
    width: 80px;
    height: 80px;
  }
}
</style>
