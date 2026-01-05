<!-- LapnetAIHubHero.vue -->
<template>
  <section ref="root" class="hubHero">
    <!-- background -->
    <div class="bg" aria-hidden="true">
      <div class="bgRadial"></div>
      <div class="bgGrid"></div>
      <div class="bgNoise"></div>
    </div>

    <!-- header -->
    <div class="head">
      <h2 class="kicker">{{ kicker }}</h2>
      <h1 class="title">
        {{ title }}
        <span class="titleAccent">{{ titleAccent }}</span>
      </h1>
      <p class="desc">{{ description }}</p>
    </div>

    <!-- stage -->
    <div ref="stage" class="stage">
      <!-- SVG wires -->
      <svg
        ref="svgEl"
        class="wires"
        :viewBox="`0 0 ${vb.w} ${vb.h}`"
        preserveAspectRatio="none"
        aria-hidden="true"
      >
        <defs>
          <filter id="wireGlow" x="-50%" y="-50%" width="200%" height="200%">
            <feGaussianBlur stdDeviation="3.4" result="blur" />
            <feColorMatrix
              in="blur"
              type="matrix"
              values="
                1 0 0 0 0
                0 1 0 0 0
                0 0 1 0 0
                0 0 0 18 -8"
              result="glow"
            />
            <feMerge>
              <feMergeNode in="glow" />
              <feMergeNode in="SourceGraphic" />
            </feMerge>
          </filter>

          <linearGradient id="wireBase" x1="0" y1="0" x2="1" y2="0">
            <stop offset="0%" stop-color="rgba(255,255,255,0.06)" />
            <stop offset="50%" stop-color="rgba(170,140,255,0.12)" />
            <stop offset="100%" stop-color="rgba(255,255,255,0.06)" />
          </linearGradient>
        </defs>

        <!-- base wires -->
        <path v-for="(d, i) in paths" :key="'b'+i" :d="d" class="wire" />
        <!-- running glow -->
        <path v-for="(d, i) in paths" :key="'g'+i" :d="d" class="wireGlow" />
      </svg>

      <!-- left column -->
      <div class="col colLeft">
        <button
          v-for="(p, idx) in leftProducts"
          :key="p.title"
          :ref="(el) => setLeftRef(el, idx)"
          class="card"
          type="button"
          @click="go(p.to)"
        >
          <div class="cardTop">
            <span class="chip">{{ p.tag }}</span>
            <span class="dot" aria-hidden="true"></span>
          </div>
          <div class="cardTitle">{{ p.title }}</div>
          <div class="cardDesc">{{ p.desc }}</div>
        </button>
      </div>

      <!-- center orb -->
      <div class="center">
        <div ref="orb" class="orb">
          <div class="orbGlow" aria-hidden="true"></div>
          <div class="orbRing" aria-hidden="true"></div>
          <img class="orbLogo" :src="logoSrc" :alt="companyName" />
        </div>

        <div class="centerMeta">
          <div class="company">{{ companyName }}</div>
          <div class="tagline">{{ companyTagline }}</div>
        </div>
      </div>

      <!-- right column -->
      <div class="col colRight">
        <button
          v-for="(p, idx) in rightProducts"
          :key="p.title"
          :ref="(el) => setRightRef(el, idx)"
          class="card"
          type="button"
          @click="go(p.to)"
        >
          <div class="cardTop">
            <span class="chip">{{ p.tag }}</span>
            <span class="dot" aria-hidden="true"></span>
          </div>
          <div class="cardTitle">{{ p.title }}</div>
          <div class="cardDesc">{{ p.desc }}</div>
        </button>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, reactive, onMounted, onBeforeUnmount, onBeforeUpdate, nextTick, type ComponentPublicInstance } from "vue";
import { useRouter } from "vue-router";
import gsap from "gsap";

type Product = { title: string; desc: string; tag: string; to: string };

const props = withDefaults(
  defineProps<{
    logoSrc?: string;
    companyName?: string;
    companyTagline?: string;

    kicker?: string;
    title?: string;
    titleAccent?: string;
    description?: string;
  }>(),
  {
    logoSrc: "/logolapnet/fullcircle.png",
    companyName: "LAPNET",
    companyTagline: "Lao National Payment Network CO., LTD",

  
    title: "ຜະລິດຕະພັນ ແລະ ການບໍລິການ ທັງໝົດຂອງ ",
    titleAccent: "LAPNet",
    description:
      "ບໍລິສັດ ລາວເນ ເຊີນນໍ ເພເມັ້ນ ເນັດເວີກ ຈຳກັດ (LAPNet)  ເປັນຜູ້ໃຫ້ບໍລິການລະບົບ ຊໍາລະທຸລະກໍາຍ່ອຍທົ່ວໄປ ຕາມທີ່ໄດ້ກໍານົດໄວ້ໃນກົດໝາຍວ່າດ້ວຍລະບົບການຊໍາລະ. ຊຶ່ງເຮັດໜ້າທີ່ເປັນໂຕກາງໃນການ ເຊື່ອມໂຍງກັບທຸກຂະແໜງການຊຳລະທີ່ເປັນທະນາຄານ, ສະຖາບັນການເງິນ ແລະ ຜູ້ໃຫ້ບໍລິການ ການຊຳລະແຫ່ງດຽວໃນ ສປປ ລາວ;",
  }
);

const router = useRouter();

const logoSrc = props.logoSrc;
const companyName = props.companyName;
const companyTagline = props.companyTagline;

const kicker = props.kicker;
const title = props.title;
const titleAccent = props.titleAccent;
const description = props.description;

// ✅ example products (edit as you want)
const leftProducts = ref<Product[]>([
  {
    title: "ກວດຍອດເງິນຂ້າມທະນາຄານຜ່ານຕູ້ ATM",
    desc: "ການກວດຍອດເງິນຂ້າມທະນາຄານຜ່ານຕູ້ ATM ແມ່ນນຳໃຊ້ລະບົບ ບັດທະນາຄານຮ່ວມກັນ (LAPS) ໃນການເຊື່ອມໂຍງຕູ້ເອທີເອັມຂອງແຕ່ລະທະນາຄານທີ່ເປັນສະມາຊິກ ຂອງບໍລິສັດລາວ ເນເຊີນນໍເພເມັ້ນ ເນັດເວີກ.",
    tag: "ATM",
    to: "/products_service/atm-inquiry",
  },
  {
    title: "ຖອນເງິນສົດຂ້າມທະນາຄານຜ່ານຕູ້ ATM",
    desc: "ການຖອນເງິນສົດຜ່ານຕູ້ເອທີເອັມ ແມ່ນນຳໃຊ້ລະບົບ ບັດທະນາຄານຮ່ວມກັນ (LAPS) ໃນການເຊື່ອມໂຍງຕູ້ເອທີເອັມຂອງແຕ່ລະທະນາຄານທີ່ເປັນສະມາຊິກຂອງ ບໍລິສັດລາວ ເນເຊີນນໍເພເມັ້ນ ເນັດເວີກ.",
    tag: "ATM",
    to: "/products_service/atm-cash-withdraw",
  },
  {
    title: "ໂອນເງິນຂ້າມທະນາຄານຜ່ານຕູ້ ATM",
    desc: "ການໂອນເງິນຂ້າມທະນາຄານຜ່ານຕູ້ ATM ແມ່ນນຳໃຊ້ລະບົບ ບັດທະນາຄານຮ່ວມກັນ (LAPS) ໃນການເຊື່ອມໂຍງຕູ້ເອທີເອັມຂອງແຕ່ລະທະນາຄານທີ່ເປັນສະມາຊິກຂອງ ບໍລິສັດລາວ ເນເຊີນນໍເພເມັ້ນ ເນັດເວີກ.",
    tag: "ATM",
    to: "/products_service/atm-transfer",
  },
]);

const rightProducts = ref<Product[]>([
  {
    title: "ໂອນເງິນຂ້າມທະນາຄານເທິງມືຖື",
    desc: "ການໂອນເງິນຂ້າມທະນາຄານເທິງມືຖື (Fund Transfer via Mobile Application) ໂດຍນຳໃຊ້ລະບົບ ຊຳລະຂ້າມທະນາຄານເທິງມືຖື (LMPS) ໃນການເຊື່ອມໂຍງຜ່ານ Applications ຂອງແຕ່ລະທະນາຄານທີ່ເປັນສະມາຊິກຂອງບໍລິສັດລາວ ເນເຊີນນໍເພເມັ້ນ ເນັດເວີກ.",
    tag: "Mobile Banking",
    to: "/products_service/mobile-transfer",
  },
  {
    title: "ຊໍາລະຂ້າມທະນາຄານຜ່ານ LAPNet",
    desc: "ການຊໍາລະຂ້າມທະນາຄານຜ່ານ LAPNet LAOQR ໂດຍນຳໃຊ້ລະບົບ ຊຳລະຂ້າມທະນາຄານເທິງມືຖື (LMPS) ໃນການເຊື່ອມໂຍງຜ່ານ Applications ຂອງແຕ່ລະທະນາຄານທີ່ເປັນສະມາຊິກຂອງບໍລິສັດ LAPNet.",
    tag: "Mobile Banking",
    to: "/products_service/qr-payment",
  },
  {
    title: "ຊຳລະຂ້າມແດນຜ່ານ QR CODE",
    desc: "ບໍລິສັດ LAPNet ໄດ້ຕໍ່ຍອດການພັດທະນາລະບົບຊຳລະຂ້າມທະນາຄານເທິງມືຖື (LMPS) ເພື່ອຂະຫຍາຍຜະລິດຕະພັນ ຊຳລະຂ້າມທະນາຄານ ໃຫ້ສາມາດຮອງຮັບການ ຊຳລະ ຄ່າສິນຄ້າ - ບໍລິການ ລະຫວ່າງປະເທດ.",
    tag: "Mobile Banking",
    to: "/products_service/crossborder",
  },
]);

const go = (to: string) => router.push(to);

// Refs
const root = ref<HTMLElement | null>(null);
const stage = ref<HTMLElement | null>(null);
const orb = ref<HTMLElement | null>(null);
const svgEl = ref<SVGSVGElement | null>(null);

// Collect multiple refs
const leftRefs = ref<HTMLElement[]>([]);
const rightRefs = ref<HTMLElement[]>([]);

onBeforeUpdate(() => {
  leftRefs.value = [];
  rightRefs.value = [];
});

const toHTMLElement = (el: Element | ComponentPublicInstance | null): HTMLElement | null => {
  if (!el) return null;
  // native DOM element
  if ((el as Element).getBoundingClientRect) return el as HTMLElement;
  // Vue component instance -> try its root $el
  const vm = el as ComponentPublicInstance & { $el?: Element | null };
  if (vm.$el && (vm.$el as Element).getBoundingClientRect) return vm.$el as HTMLElement;
  return null;
};

const setLeftRef = (el: Element | ComponentPublicInstance | null, idx: number) => {
  const h = toHTMLElement(el);
  if (h) leftRefs.value[idx] = h;
};

const setRightRef = (el: Element | ComponentPublicInstance | null, idx: number) => {
  const h = toHTMLElement(el);
  if (h) rightRefs.value[idx] = h;
};

// SVG viewBox + paths
const vb = reactive({ w: 1000, h: 520 });
const paths = ref<string[]>([]);

let ro: ResizeObserver | null = null;
let rafId: number | null = null;
let signalTweens: gsap.core.Tween[] = [];

function buildPath(sx: number, sy: number, ex: number, ey: number) {
  const dx = ex - sx;
  const dy = ey - sy;
  const bend = Math.min(220, Math.max(120, Math.abs(dx) * 0.35));
  const c1x = sx + Math.sign(dx) * bend;
  const c2x = ex - Math.sign(dx) * bend;
  const c1y = sy + dy * 0.08;
  const c2y = ey - dy * 0.08;
  return `M ${sx} ${sy} C ${c1x} ${c1y}, ${c2x} ${c2y}, ${ex} ${ey}`;
}

function queueRecalc() {
  if (rafId) cancelAnimationFrame(rafId);
  rafId = requestAnimationFrame(() => {
    recalc();
  });
}

function recalc() {
  if (!stage.value || !orb.value) return;

  const sRect = stage.value.getBoundingClientRect();
  const oRect = orb.value.getBoundingClientRect();

  vb.w = Math.max(740, Math.round(sRect.width));
  vb.h = Math.max(420, Math.round(sRect.height));

  const startX = oRect.left + oRect.width / 2 - sRect.left;
  const startY = oRect.top + oRect.height / 2 - sRect.top;

  const newPaths: string[] = [];

  // left: connect to card right edge
  for (const el of leftRefs.value) {
    if (!el) continue;
    const r = el.getBoundingClientRect();
    const endX = r.right - sRect.left;
    const endY = r.top + r.height / 2 - sRect.top;
    newPaths.push(buildPath(startX, startY, endX, endY));
  }

  // right: connect to card left edge
  for (const el of rightRefs.value) {
    if (!el) continue;
    const r = el.getBoundingClientRect();
    const endX = r.left - sRect.left;
    const endY = r.top + r.height / 2 - sRect.top;
    newPaths.push(buildPath(startX, startY, endX, endY));
  }

  paths.value = newPaths;
  startSignal(); // update running glow
}

async function startSignal() {
  await nextTick();

  signalTweens.forEach((t) => t.kill());
  signalTweens = [];

  const svg = svgEl.value;
  if (!svg) return;

  const glowPaths = Array.from(svg.querySelectorAll<SVGPathElement>(".wireGlow"));

  glowPaths.forEach((p, i) => {
    const len = p.getTotalLength();

    const seg = Math.max(18, Math.min(36, len * 0.11));
    p.style.strokeDasharray = `${seg} ${len + seg}`;
    p.style.strokeDashoffset = "0";

    const tween = gsap.to(p, {
      strokeDashoffset: -(len + seg + 60),
      duration: 1.15 + Math.random() * 0.8,
      ease: "none",
      repeat: -1,
      delay: i * 0.12,
    });

    signalTweens.push(tween);
  });
}

onMounted(async () => {
  await nextTick();
  recalc();

  ro = new ResizeObserver(() => queueRecalc());
  if (stage.value) ro.observe(stage.value);

  const onScroll = () => queueRecalc();
  window.addEventListener("scroll", onScroll, true);

  // entrance animations
  const reduce = window.matchMedia?.("(prefers-reduced-motion: reduce)")?.matches ?? false;

  const ctx = gsap.context(() => {
    gsap.from(".head", { y: 14, opacity: 0, duration: 0.8, ease: "power3.out" });

    gsap.from(".card", {
      y: 14,
      opacity: 0,
      duration: 0.75,
      ease: "power3.out",
      stagger: 0.08,
      delay: 0.08,
    });

    gsap.from(".orb", { scale: 0.92, opacity: 0, duration: 0.9, ease: "power3.out", delay: 0.15 });

    if (!reduce) {
      gsap.to(".orbRing", { rotation: 360, duration: 12, repeat: -1, ease: "none" });
      gsap.to(".orbGlow", { opacity: 0.95, duration: 1.2, yoyo: true, repeat: -1, ease: "sine.inOut" });
      gsap.to(".stage", { y: -4, duration: 3.8, yoyo: true, repeat: -1, ease: "sine.inOut" });
    }
  }, root);

  onBeforeUnmount(() => {
    ctx.revert();
    window.removeEventListener("scroll", onScroll, true);
  });
});

onBeforeUnmount(() => {
  if (rafId) cancelAnimationFrame(rafId);
  ro?.disconnect();
  ro = null;
  signalTweens.forEach((t) => t.kill());
  signalTweens = [];
});
</script>

<style scoped>
/* =========================
   Base / background
========================= */
.hubHero{
  position: relative;
  padding: 56px 18px 86px;
  overflow: hidden;
  color: rgba(255, 255, 255, 0.92);
  background: #07050f;
}

.bg{ position:absolute; inset:0; pointer-events:none; z-index:0; }
.bgRadial{
  position:absolute; inset:-25%;
  background:
    radial-gradient(980px 620px at 50% 10%, rgba(0, 42, 255, 0.12), transparent 62%),
    radial-gradient(980px 740px at 40% 45%, rgba(8, 0, 232, 0.18), transparent 65%),
    radial-gradient(820px 660px at 62% 78%, rgba(0, 42, 255, 0.12), transparent 68%);
  filter: blur(2px);
}
.bgGrid{
  position:absolute; inset:0;
  background-image:
    linear-gradient(rgba(38, 0, 255, 0.05) 1px, transparent 1px),
    linear-gradient(90deg, rgba(0, 4, 255, 0.05) 1px, transparent 1px);
  background-size: 58px 58px;
  opacity: 0.08;
  mask-image: radial-gradient(closest-side at 50% 20%, rgba(0,0,0,1), rgba(0,0,0,0));
}
.bgNoise{
  position:absolute; inset:0;
  opacity: 0.10;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='180' height='180'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.8' numOctaves='2' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='180' height='180' filter='url(%23n)' opacity='.6'/%3E%3C/svg%3E");
  background-size: 180px 180px;
  mix-blend-mode: overlay;
}

/* =========================
   Header
========================= */
.head{
  position: relative;
  z-index: 2;
  max-width: 1380px;
  margin: 0 auto 26px;
  text-align: center;
}

.kicker{
  font-size: 12px;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: rgba(255,255,255,0.64);
  margin: 0 0 10px;
}

.title{
  margin: 0;
  font-size: clamp(34px, 4.3vw, 58px);
  line-height: 1.05;
  font-weight: 700;
  letter-spacing: -0.02em;
}
.titleAccent{
   background: linear-gradient(120deg, #0033ab 0%, #0078ff 40%, #00c6ff 100%);
    -webkit-background-clip: text;
    background-clip: text;
    color: transparent;
  text-shadow: 0 22px 90px rgba(0,200,255,0.14);
  margin-left: 10px;
}

.desc{
  max-width: 880px;
  
  margin: 12px auto 0;
  color: rgba(255,255,255,0.72);
  font-size: 15px;
  
  line-height: 1.7;
}

/* =========================
   Stage layout
========================= */
.stage{
  position: relative;
  z-index: 1;
  max-width: 1380px;
  margin: 22px auto 0;
  min-height: 420px;
  padding: 26px 18px;
  border-radius: 26px;
  background:
    radial-gradient(740px 520px at 50% 28%, rgba(110, 231, 255, 0.12), transparent 60%),
    radial-gradient(740px 520px at 50% 90%, rgba(13, 0, 255, 0.14), transparent 60%),
    rgba(10, 8, 18, 0.38);
  border: 1px solid rgba(255,255,255,0.10);
  backdrop-filter: blur(14px);
  box-shadow: 0 30px 110px rgba(0,0,0,0.48), inset 0 1px 0 rgba(31, 2, 255, 0.06);

  display: grid;
  grid-template-columns: 1fr auto 1fr;
  gap: 18px;
  align-items: center;
}

/* SVG wires behind cards */
.wires{
  position:absolute;
  inset:0;
  z-index: 0;
  pointer-events:none;
}

.wire{
  fill:none;
  stroke: url(#wireBase);
  stroke-width: 1.2;
  opacity: 0.95;
}
.wireGlow{
  fill:none;
  stroke: rgba(255,255,255,0.95);
  stroke-width: 2.8;
  opacity: 0.86;
  filter: url(#wireGlow);
  stroke-linecap: round;
  stroke-linejoin: round;
}

/* =========================
   Columns + cards
========================= */
.col{
  position: relative;
  z-index: 2;
  display: flex;
  flex-direction: column;
  gap: 12px;
}
.colLeft{ align-items: flex-end; }
.colRight{ align-items: flex-start; }

.card{
  width: min(560px, 100%);
  text-align: left;
  border-radius: 18px;
  padding: 14px 14px 14px;
  border: 1px solid rgba(255,255,255,0.12);
  background:
    radial-gradient(320px 220px at 20% 0%, rgba(0, 34, 255, 0.1), transparent 60%),
    radial-gradient(320px 220px at 80% 100%, rgba(124, 58, 237, 0.12), transparent 60%),
    rgba(8, 7, 14, 0.55);
  box-shadow: 0 18px 60px rgba(0,0,0,0.35);
  cursor: pointer;
  transition: transform .18s ease, box-shadow .18s ease, border-color .18s ease;
}

.card:hover{
  transform: translateY(-2px);
  border-color: rgba(255,255,255,0.18);
  box-shadow: 0 22px 70px rgba(0,0,0,0.44);
}

.cardTop{
  display:flex;
  align-items:center;
  justify-content: space-between;
  gap: 10px;
  margin-bottom: 10px;
}

.chip{
  font-size: 11px;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  padding: 6px 10px;
  border-radius: 999px;
  border: 1px solid rgba(255,255,255,0.12);
  background: rgba(255,255,255,0.06);
  color: rgba(255,255,255,0.72);
}

.dot{
  width: 9px; height: 9px; border-radius: 999px;
  background: radial-gradient(circle at 30% 20%, rgba(255,255,255,1), rgba(0,200,255,0.55));
  box-shadow: 0 0 14px rgba(255,255,255,0.45), 0 0 28px rgba(0,200,255,0.28);
}

.cardTitle{
  font-size: 15px;
  font-weight: 800;
  color: rgba(255,255,255,0.92);
  margin-bottom: 6px;
}

.cardDesc{
  font-size: 13px;
  line-height: 1.6;
  
  color: rgba(255,255,255,0.68);
}

/* =========================
   Center orb
========================= */
.center{
  position: relative;
  z-index: 3;
  display: grid;
  place-items: center;
  gap: 10px;
  padding: 10px 6px;
}

.orb{
  position: relative;
  width: 118px;
  height: 118px;
  border-radius: 999px;
  display: grid;
  place-items: center;
  background:
    radial-gradient(circle at 30% 20%, rgba(255,255,255,0.22), transparent 40%),
    radial-gradient(circle at 50% 60%, rgba(0, 47, 255, 0.16), rgba(8,7,14,0.85));
  border: 1px solid rgba(255,255,255,0.12);
  box-shadow:
    0 0 40px rgba(0, 64, 255, 0.18),
    0 0 90px rgba(124,58,237,0.16);
}

.orbLogo{
  width: 120px;
  height: 120px;
  object-fit: contain;
  filter: drop-shadow(0 8px 26px rgba(255,255,255,0.18));
}

.orbGlow{
  position:absolute;
  inset:-18px;
  border-radius: inherit;
  background: radial-gradient(circle at 50% 50%, rgba(1, 52, 255, 0.2), transparent 60%);
  opacity: 0.75;
  pointer-events: none;
}

.orbRing{
  position:absolute;
  inset:-10px;
  border-radius: inherit;
  border: 1px solid rgba(255,255,255,0.14);
  box-shadow: inset 0 0 0 1px rgba(0,200,255,0.10);
  pointer-events: none;
}

.centerMeta{
  text-align:center;
}

.company{
  font-size: 12px;
  letter-spacing: 0.18em;
  font-weight: 800;
  color: rgba(255,255,255,0.80);
}

.tagline{
  font-size: 12px;
  color: rgba(255,255,255,0.62);
}

/* =========================
   Responsive
========================= */
@media (max-width: 980px){
  .stage{
    grid-template-columns: 1fr;
    gap: 14px;
    padding: 18px 14px;
    min-height: 520px;
  }

  .colLeft, .colRight{
    align-items: stretch;
  }

  .center{
    order: -1;
    margin-bottom: 6px;
  }

  /* optional: wires still work, but you can hide if you want */
  /* .wires{ display:none; } */

  .card{
    width: 100%;
  }
}

@media (max-width: 520px){
  .hubHero{ padding: 44px 14px 70px; }
  .orb{ width: 120px; height: 120px; }
  .orbLogo{ width: 120px; height: 120px; }
}

@media (max-width: 889px){
  .wires{
    display: none !important;
  }
}
</style>
