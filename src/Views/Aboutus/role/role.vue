<template>
  <main_navbar title="ໂຄງຮ່າງການຈັດຕັ້ງ" :breadcrumb="[
    'ໜ້າຫຼັກ',
    'ກ່ຽວກັບພວກເຮົາ',
    'ໂຄງຮ່າງການຈັດຕັ້ງ'
  ]" background-image="/aboutus/navigatormission-bg.png" />

  <section class="hero">
    <!-- background layers -->
    <div class="hero-bg">
      <div class="hero-gradient"></div>

      <!-- 🎥 Video background overlay -->
      <video class="hero-video-overlay" autoplay muted loop playsinline>
        <source src="/videos/Rolebackground.mp4" type="video/mp4" />
        Your browser does not support the video tag.
      </video>

      <!-- geometric mesh / grid -->
      <div class="hero-mesh"></div>

      <!-- radial glow orbs -->
      <div class="hero-orb orb-1"></div>
      <div class="hero-orb orb-2"></div>
      <div class="hero-orb orb-3"></div>
    </div>

    <div class="hero-inner">
      <!-- Glassmorphism card -->
      <div ref="glassCard" class="hero-card">
        <div class="hero-pill">
          <span class="pill-dot"><img src="/logolapnet/fullcircle.png" alt=""></span>
          <span class="pill-text">Lao National Payment Network CO., LTD</span>
        </div>

        <h1 class="hero-title">
          ພາລະບົດບາດ.
        </h1>

        <p class="hero-subtitle">
          ບໍລິສັດ ລາວເນ ເຊີນນໍ ເພເມັ້ນ ເນັດເວີກ ຈຳກັດ (LAPNet) ມີພາລະບົດບາດ ເປັນຜູ້ຄວບຄຸມລະບົບຊໍາລະ
          ທົ່ວໄປຕາມການອະນຸຍາດຂອງ ທະນາຄານແຫ່ງ ສປປ ລາວ. LAPNet ໃຫ້ບໍລິການ ຄື: ລະບົບບັດທະນາຄານຮ່ວມກັນ (Lao ATM Pool
          Switching: LAPS) ແລະ ລະບົບຊໍາລະຂ້າມທະນາຄານເທິງມືຖື (Lao Mobile Payment System: LMPS)
          ໂດຍເຮັດໜ້າທີ່ເປັນຕົວກາງໃນການເຊື່ອມໂຍງກັບທຸກຂະແໜງການຊຳລະ ບໍ່ວ່າຈະເປັນທະນາຄານ, ສະຖາບັນການເງິນ ຫຼື ບໍລິສັດ
          FinTech ຫຼື ຜູ້ໃຫ້ບໍລິການ ການຊໍາລະອື່ນໆ ໃນ ສປປ ລາວ ພ້ອມທັງເປັນຜູ້ໄລ່ລຽງບັນດາທຸລະກໍາທາງການເງິນເລົ່ານັ້ນ
          ແລ້ວສົ່ງໄປສູນຊໍາລະສຸດທ້າຍ ທີ່ LaPASS ທະນາຄານແຫ່ງ ສປປ ລາວ.
        </p>

        <div class="hero-actions">
          <router-link to="/">
            <button class="btn-primary">
              ໜ້າຫຼັກ
            </button>
          </router-link>
          <router-link to="/products_service/allproduct">
            <button class="btn-ghost">
              ຜະລິດຕະພັນ ແລະ ການບໍລິການ
            </button>
          </router-link>
        </div>

        <div class="hero-metrics">
          <div class="metric">
            <span class="metric-value">21</span>
            <span class="metric-label">ສະມາຊິກ</span>
          </div>
          <div class="metric">
            <span class="metric-value">19</span>
            <span class="metric-label">ທະນາຄານສະມາຊິກ</span>
          </div>
          <div class="metric">
            <span class="metric-value">2</span>
            <span class="metric-label">Fintech</span>
          </div>
        </div>
      </div>

      <!-- Right: network / mesh visual -->
      <div class="hero-visual">
        <svg class="network-svg" viewBox="0 0 420 420" xmlns="http://www.w3.org/2000/svg">
          <circle class="net-ring" cx="210" cy="210" r="150" />
          <circle class="net-ring net-ring-inner" cx="210" cy="210" r="90" />

          <g class="net-lines">
            <line x1="210" y1="60" x2="90" y2="150" />
            <line x1="90" y1="150" x2="110" y2="290" />
            <line x1="110" y1="290" x2="210" y2="360" />
            <line x1="210" y1="360" x2="310" y2="290" />
            <line x1="310" y1="290" x2="330" y2="150" />
            <line x1="330" y1="150" x2="210" y2="60" />

            <line x1="210" y1="60" x2="210" y2="210" />
            <line x1="90" y1="150" x2="210" y2="210" />
            <line x1="110" y1="290" x2="210" y2="210" />
            <line x1="310" y1="290" x2="210" y2="210" />
            <line x1="330" y1="150" x2="210" y2="210" />
          </g>

          <g class="net-nodes">
            <circle cx="210" cy="60" r="6" />
            <circle cx="90" cy="150" r="6" />
            <circle cx="110" cy="290" r="6" />
            <circle cx="210" cy="360" r="6" />
            <circle cx="310" cy="290" r="6" />
            <circle cx="330" cy="150" r="6" />
            <circle class="net-node-core" cx="210" cy="210" r="8" />
          </g>
        </svg>

        <!-- Glass chip stack -->
        <div ref="chipStack" class="chip-stack">
          <router-link class="chip-link" to="/products_service/atm-inquiry">
            <div class="chip-row">
              <span class="chip-dot"></span>
              <span class="chip-label">ກວດຍອດເງິນຂ້າມທະນາຄານຜ່ານຕູ້ ATM</span>
              <span class="chip-value">01</span>
            </div>
          </router-link>

          <router-link class="chip-link" to="/products_service/atm-cash-withdraw">
            <div class="chip-row">
              <span class="chip-dot"></span>
              <span class="chip-label">ຖອນເງິນສົດຂ້າມທະນາຄານຜ່ານຕູ້ ATM</span>
              <span class="chip-value">02</span>
            </div>
          </router-link>

          <router-link class="chip-link" to="/products_service/atm-transfer">
            <div class="chip-row">
              <span class="chip-dot"></span>
              <span class="chip-label">ໂອນເງິນຂ້າມທະນາຄານຜ່ານຕູ້ ATM</span>
              <span class="chip-value">03</span>
            </div>
          </router-link>

          <router-link class="chip-link" to="/products_service/mobile-transfer">
            <div class="chip-row">
              <span class="chip-dot"></span>
              <span class="chip-label">ໂອນເງິນຂ້າມທະນາຄານເທິງມືຖື</span>
              <span class="chip-value">04</span>
            </div>
          </router-link>

          <router-link class="chip-link" to="/products_service/qr-payment">
            <div class="chip-row">
              <span class="chip-dot"></span>
              <span class="chip-label">ຊໍາລະຂ້າມທະນາຄານຜ່ານ LAPNet</span>
              <span class="chip-value">05</span>
            </div>
          </router-link>

          <router-link class="chip-link" to="/products_service/crossborder">
            <div class="chip-row">
              <span class="chip-dot"></span>
              <span class="chip-label">ຊຳລະຂ້າມແດນໃນຮູບແບບ QR CODE </span>
              <span class="chip-value">06</span>
            </div>
          </router-link>
        </div>
      </div>
    </div>
  </section>

  <secondfooter />
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { gsap } from 'gsap'
import main_navbar from '../../../components/miannavbar/main_navbar.vue'
import secondfooter from '../../../components/footer/mainfooter/secondfooter.vue'

const glassCard = ref(null)
const chipStack = ref(null)

onMounted(() => {
  // main glass card float-in
  if (glassCard.value) {
    gsap.from(glassCard.value, {
      y: 40,
      opacity: 0,
      duration: 1.1,
      ease: 'power3.out'
    })
  }

  // chip stack float-in
  if (chipStack.value) {
    gsap.from(chipStack.value, {
      y: 30,
      opacity: 0,
      duration: 1,
      ease: 'power3.out',
      delay: 0.2
    })
  }

  // subtle looping float
  if (glassCard.value) {
    gsap.to(glassCard.value, {
      y: -8,
      duration: 4,
      ease: 'sine.inOut',
      yoyo: true,
      repeat: -1
    })
  }

  if (chipStack.value) {
    gsap.to(chipStack.value, {
      y: -6,
      duration: 3.5,
      ease: 'sine.inOut',
      yoyo: true,
      repeat: -1,
      delay: 0.5
    })
  }
})
</script>

<style scoped>
.hero {
  position: relative;
  width: 100%;
  min-height: 70vh;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 32px 24px;
  box-sizing: border-box;
  color: #f9fafb;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'SF Pro Text', sans-serif;
}

/* BACKGROUND LAYERS */
.hero-bg {
  position: absolute;
  inset: 0;
  pointer-events: none;
  overflow: hidden;
}

.hero-gradient {
  position: absolute;
  inset: 0;
  background: linear-gradient(to right, #031239 0%, #093584 100%);
}

/* 🎥 Video background overlay */
.hero-video-overlay {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  mix-blend-mode: soft-light;
  opacity: 0.7;
  pointer-events: none;
}

/* simple mesh / grid */
.hero-mesh {
  position: absolute;
  inset: 0;
  background-image:
    linear-gradient(rgba(255, 255, 255, 0.08) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255, 255, 255, 0.08) 1px, transparent 1px);
  background-size: 80px 80px;
  opacity: 0.4;
}

/* glowing orbs */
.hero-orb {
  position: absolute;
  border-radius: 999px;
  filter: blur(40px);
  opacity: 0.9;
}

.orb-1 {
  width: 260px;
  height: 260px;
  background: radial-gradient(circle, rgba(56, 189, 248, 0.9), transparent 60%);
  top: -40px;
  left: 10%;
}

.orb-2 {
  width: 220px;
  height: 220px;
  background: radial-gradient(circle, rgba(129, 140, 248, 0.9), transparent 60%);
  bottom: -60px;
  right: 8%;
}

.orb-3 {
  width: 200px;
  height: 200px;
  background: radial-gradient(circle, rgba(52, 211, 153, 0.9), transparent 60%);
  bottom: 10%;
  left: 25%;
}

/* CONTENT LAYOUT */
.hero-inner {
  position: relative;
  max-width: 1200px;
  width: 100%;
  display: grid;
  grid-template-columns: minmax(0, 3fr) minmax(0, 2.5fr);
  gap: 48px;
  align-items: center;
  z-index: 1;
}

/* GLASS CARD (LEFT) */
.hero-card {
  position: relative;
  padding: 28px 30px;
  border-radius: 26px;
  background: radial-gradient(circle at top left, rgba(255, 255, 255, 0.18), rgba(12, 18, 44, 0.8));
  border: 1px solid rgba(255, 255, 255, 0.16);
  box-shadow:
    0 24px 60px rgba(15, 23, 42, 0.9),
    0 0 0 1px rgba(148, 163, 184, 0.35);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
}

/* small accent line on top left */
.hero-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 24px;
  width: 96px;
  height: 2px;
  border-radius: 999px;
  background: linear-gradient(to right, #22d3ee, #a855f7);
  opacity: 0.9;
}

/* pill tag */
.hero-pill {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 4px 10px 4px 4px;
  border-radius: 999px;
  background: rgba(15, 23, 42, 0.8);
  border: 1px solid rgba(148, 163, 184, 0.4);
  margin-bottom: 14px;
}

.pill-dot {
  width: 30px;
  height: 30px;
  border-radius: 999px;
  background: radial-gradient(circle, #005eff 0%, #00beee 40%, transparent 70%);
  box-shadow: 0 0 12px rgba(0, 153, 255, 0.8);
}

.pill-text {
  font-size: 12px;
  letter-spacing: 0.04em;
  text-transform: uppercase;
  color: #e5e7eb;
}

/* headings */
.hero-title {
  font-size: clamp(32px, 4vw, 42px);
  line-height: 1.15;
  margin: 0 0 14px;
  font-weight: 650;
}

.accent {
  background: linear-gradient(to right, #f9fafb, #a5b4fc);
  -webkit-background-clip: text;
  color: transparent;
}

.hero-subtitle {
  margin: 0 0 20px;
  font-size: 14px;
  line-height: 1.6;
  color: #e5e7eb;
  opacity: 0.9;
}

/* actions */
.hero-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-bottom: 20px;
}

.btn-primary {
  padding: 10px 18px;
  border-radius: 999px;
  border: none;
  cursor: pointer;
  font-size: 14px;
  font-weight: 500;
  color: #ffffff;
  background: linear-gradient(to right, #22d3ee, #6366f1);
  box-shadow: 0 16px 40px rgba(37, 99, 235, 0.6);
}

.btn-primary:hover {
  filter: brightness(1.05);
}

.btn-ghost {
  padding: 9px 16px;
  border-radius: 999px;
  border: 1px solid rgba(156, 163, 175, 0.5);
  background: rgba(15, 23, 42, 0.7);
  color: #e5e7eb;
  font-size: 14px;
  cursor: pointer;
}

.btn-ghost:hover {
  border-color: rgba(209, 213, 219, 0.9);
}

/* metrics */
.hero-metrics {
  display: flex;
  flex-wrap: wrap;
  gap: 18px;
}

.metric {
  display: flex;
  flex-direction: column;
}

.metric-value {
  font-size: 18px;
  font-weight: 600;
}

.metric-label {
  font-size: 11px;
  text-transform: uppercase;
  letter-spacing: 0.09em;
  color: #9ca3af;
}

/* RIGHT VISUAL AREA */
.hero-visual {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
}

.network-svg {
  width: 100%;
  max-width: 360px;
  filter: drop-shadow(0 16px 40px rgba(15, 23, 42, 0.9));
}

/* rings & lines */
.net-ring {
  fill: none;
  stroke: rgba(191, 219, 254, 0.5);
  stroke-width: 1.4;
}

.net-ring-inner {
  stroke: rgba(96, 165, 250, 0.9);
  stroke-dasharray: 4 8;
}

.net-lines line {
  stroke: rgba(191, 219, 254, 0.7);
  stroke-width: 1.4;
  stroke-linecap: round;
}

/* nodes */
.net-nodes circle {
  fill: #22d3ee;
  stroke: rgba(15, 23, 42, 0.95);
  stroke-width: 1.8;
}

.net-node-core {
  fill: #38bdf8;
  box-shadow: 0 0 16px rgba(56, 189, 248, 0.9);
}

/* =========================
   ✨ MODERN CHIP-STACK HOVER
   ========================= */

/* router-link style reset */
.chip-stack :deep(a),
.chip-link {
  display: block;
  text-decoration: none;
  color: inherit;
  -webkit-tap-highlight-color: transparent;
}

.chip-stack {
  --stack-x: 18%;
  position: absolute;
  right: 0;
  bottom: 16px;
  transform: translateX(var(--stack-x));
  padding: 12px;
  border-radius: 20px;
  background: rgba(15, 23, 42, 0.78);
  border: 1px solid rgba(148, 163, 184, 0.55);
  backdrop-filter: blur(18px);
  -webkit-backdrop-filter: blur(18px);
  min-width: 240px;

  box-shadow:
    0 22px 60px rgba(2, 6, 23, 0.65),
    0 0 0 1px rgba(148, 163, 184, 0.18);
  transition:
    transform 320ms cubic-bezier(0.2, 0.9, 0.2, 1),
    box-shadow 320ms cubic-bezier(0.2, 0.9, 0.2, 1),
    border-color 320ms ease,
    background 320ms ease;
  will-change: transform;
}

/* gradient “border” ring (nice premium look) */
.chip-stack::before {
  content: '';
  position: absolute;
  inset: -1px;
  border-radius: 22px;
  padding: 1px;
  background: linear-gradient(135deg, rgba(34, 211, 238, 0.75), rgba(99, 102, 241, 0.55), rgba(168, 85, 247, 0.45));
  opacity: 0.35;
  pointer-events: none;

  /* mask to show only border area */
  -webkit-mask:
    linear-gradient(#000 0 0) content-box,
    linear-gradient(#000 0 0);
  -webkit-mask-composite: xor;
  mask-composite: exclude;
}

.chip-stack::after {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: 20px;
  pointer-events: none;
  background: radial-gradient(circle at 20% 10%, rgba(255, 255, 255, 0.18), transparent 35%),
              radial-gradient(circle at 80% 90%, rgba(56, 189, 248, 0.14), transparent 45%);
  opacity: 0.55;
  transition: opacity 320ms ease;
}

/* stack hover (lift + glow) */
.chip-stack:hover {
  transform: translateX(var(--stack-x)) translateY(-10px) scale(1.015);
  border-color: rgba(191, 219, 254, 0.75);
  background: rgba(15, 23, 42, 0.72);
  box-shadow:
    0 30px 80px rgba(2, 6, 23, 0.75),
    0 0 0 1px rgba(148, 163, 184, 0.22),
    0 0 40px rgba(56, 189, 248, 0.18);
}

.chip-stack:hover::after {
  opacity: 0.8;
}

/* rows */
.chip-row {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 10px;
  padding: 10px 12px;
  border-radius: 14px;
  overflow: hidden;

  background: rgba(2, 6, 23, 0.18);
  border: 1px solid rgba(148, 163, 184, 0.16);

  transition:
    transform 240ms cubic-bezier(0.2, 0.9, 0.2, 1),
    border-color 240ms ease,
    background 240ms ease,
    box-shadow 240ms ease;
}

/* spacing between rows */
.chip-link + .chip-link {
  margin-top: 8px;
}

/* hover overlay gradient + left accent bar */
.chip-row::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(90deg,
    rgba(34, 211, 238, 0.18) 0%,
    rgba(99, 102, 241, 0.12) 40%,
    rgba(168, 85, 247, 0.08) 100%);
  opacity: 0;
  transition: opacity 240ms ease;
  pointer-events: none;
}

.chip-row::after {
  content: '';
  position: absolute;
  left: 0;
  top: 10px;
  bottom: 10px;
  width: 3px;
  border-radius: 999px;
  background: linear-gradient(to bottom, #22d3ee, #6366f1);
  opacity: 0;
  transform: translateX(-6px);
  transition:
    opacity 240ms ease,
    transform 240ms ease;
  pointer-events: none;
}

/* hover + focus styles */
.chip-link:hover .chip-row,
.chip-link:focus-visible .chip-row {
  transform: translateX(4px);
  border-color: rgba(191, 219, 254, 0.45);
  background: rgba(2, 6, 23, 0.26);
  box-shadow:
    0 14px 30px rgba(2, 6, 23, 0.45),
    0 0 0 1px rgba(56, 189, 248, 0.15);
}

.chip-link:hover .chip-row::before,
.chip-link:focus-visible .chip-row::before {
  opacity: 1;
}

.chip-link:hover .chip-row::after,
.chip-link:focus-visible .chip-row::after {
  opacity: 1;
  transform: translateX(0);
}

/* dot pulse */
.chip-dot {
  width: 10px;
  height: 10px;
  border-radius: 999px;
  background: radial-gradient(circle, #22c55e, transparent 60%);
  box-shadow: 0 0 10px rgba(34, 197, 94, 0.85);
  flex: 0 0 auto;
  transform: translateZ(0);
  transition: transform 240ms cubic-bezier(0.2, 0.9, 0.2, 1), box-shadow 240ms ease;
}

.chip-link:hover .chip-dot,
.chip-link:focus-visible .chip-dot {
  transform: scale(1.25);
  box-shadow:
    0 0 14px rgba(34, 197, 94, 0.85),
    0 0 26px rgba(34, 197, 94, 0.35);
}

/* text */
.chip-label {
  flex: 1;
  color: #e5e7eb;
  font-size: 13px;
  line-height: 1.25;
  opacity: 0.95;
}

/* value badge */
.chip-value {
  font-weight: 650;
  font-size: 12px;
  letter-spacing: 0.08em;
  color: #c7d2fe;
  padding: 6px 10px;
  border-radius: 999px;
  background: rgba(99, 102, 241, 0.14);
  border: 1px solid rgba(99, 102, 241, 0.28);
  transition:
    transform 240ms cubic-bezier(0.2, 0.9, 0.2, 1),
    background 240ms ease,
    border-color 240ms ease,
    color 240ms ease;
}

.chip-link:hover .chip-value,
.chip-link:focus-visible .chip-value {
  transform: translateX(2px);
  background: rgba(34, 211, 238, 0.14);
  border-color: rgba(34, 211, 238, 0.35);
  color: #e0f2fe;
}

/* reduced motion */
@media (prefers-reduced-motion: reduce) {
  .chip-stack,
  .chip-stack::after,
  .chip-row,
  .chip-row::before,
  .chip-row::after,
  .chip-dot,
  .chip-value {
    transition: none !important;
  }
}

/* RESPONSIVE */
@media (max-width: 960px) {
  .hero-inner {
    grid-template-columns: minmax(0, 1fr);
    gap: 32px;
  }

  .hero-visual {
    order: -1;
  }

  .chip-stack {
    --stack-x: 0%;
    position: static;
    transform: none;
    margin-top: 16px;
    align-self: flex-end;
  }

  .chip-stack:hover {
    transform: translateY(-8px) scale(1.01);
  }
}

@media (max-width: 640px) {
  .hero {
    padding: 24px 16px;
  }

  .hero-card {
    padding: 22px 20px;
  }

  .hero-inner {
    gap: 24px;
  }

  .network-svg {
    max-width: 280px;
  }
}

@media (max-width:785px) {
  .chip-stack {
    display: none;
  }

  .hero-visual {
    display: none;
  }
}
</style>
