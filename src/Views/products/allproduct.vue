<!-- AllProduct.vue (ปรับให้เมื่อมาจาก product2 แล้ว scroll ไป card product2 ให้อยู่กลางจอ) -->
<template>
  <navbar
    title="ຜະລິດຕະພັນ ແລະ ການບໍລິການ"
    :breadcrumb="[
      'ໜ້າຫຼັກ',
      'ຜະລິດຕະພັນ ແລະ ການບໍລິການ ທັງຫມົດຂອງ LAPNet',
    ]"
    background-image="/overlaynav/product/navigatorcontent-bg.png"
  />

  <section ref="heroEl" class="hero">
    <div class="titleallproduct" style="width: 80%; height: 13vh">
      <span>
        <p
          style="
            font-size: clamp(34px, 4vw, 54px);
            text-align: center;
            padding-top: 30px;
            color: #fff;
            font-weight: bold;
          "
        >
          ຜະລິດຕະພັນ ແລະ ການບໍລິການ ທັງຫມົດຂອງ LAPNet
        </p>
      </span>
    </div>

    <div class="boxmargin" style="width: 100%; height: 10vh"></div>

    <div class="hero__inner">
      <!-- Left -->
      <div class="left">
        <div class="pill">Lao Nationnal Payment Network</div>

        <h1 ref="titleEl" class="title">{{ activeItem.title }}</h1>
        <p ref="subtitleEl" class="subtitle">{{ activeItem.desc }}</p>

        <div class="actions">
          <router-link class="btn-read" :to="activeItem.link">
            <span class="btn-read__text">Read More</span>
            <span class="btn-read__icon">→</span>
            <span class="btn-read__shine" />
          </router-link>
        </div>

        <div class="media">
          <div class="media__frame">
            <img
              ref="imgEl"
              class="media__img"
              :src="activeItem.img"
              :alt="activeItem.title"
              draggable="false"
            />
            <div class="media__glow" />
          </div>
        </div>
      </div>

      <!-- Right -->
      <div class="right">
        <button
          v-for="(it, idx) in items"
          :key="it.id"
          :ref="(el) => (cardRefs[idx] = el)"
          class="card"
          :data-id="it.id"
          :class="{ 'is-active': idx === activeIndex }"
          type="button"
          @click="setActive(idx)"
        >
          <div class="card__top">
            <div class="dots">
              <span
                v-for="n in items.length"
                :key="n"
                class="dot"
                :class="{ 'dot--on': idx === activeIndex && n <= idx + 1 }"
              />
            </div>
            <div class="duration">{{ it.duration }}</div>
          </div>

          <div class="card__body">
            <div class="card__title">{{ it.title }}</div>
            <div class="card__desc">{{ it.desc }}</div>
          </div>

          <div class="card__border" />
        </button>
      </div>
    </div>

    <div class="boxmargin" style="width: 100%; height: 7vh"></div>
  </section>

  <secondfooter />
</template>

<script setup>
import { computed, nextTick, onMounted, ref, watch } from "vue";
import { useRoute } from "vue-router";
import gsap from "gsap";
import navbar from "../../components/miannavbar/main_navbar.vue";
import secondfooter from "../../components/footer/mainfooter/secondfooter.vue";

const route = useRoute();

const heroEl = ref(null);
const imgEl = ref(null);
const titleEl = ref(null);
const subtitleEl = ref(null);

// ✅ เก็บ element ของการ์ดแต่ละใบ (ใช้ scrollIntoView)
const cardRefs = ref([]);

const isAnimating = ref(false);

/* ✅ 6 products */
const items = [
  {
    id: 1,
    title: "ກວດຍອດເງິນຂ້າມທະນາຄານຜ່ານຕູ້ ATM",
    desc: "ການກວດຍອດເງິນຂ້າມທະນາຄານຜ່ານຕູ້ ATM ແມ່ນນຳໃຊ້ລະບົບ ແລັບໃນການເຊື່ອມໂຍງຕູ້ເອທີເອັມຂອງແຕ່ລະທະນາຄານທີ່ເປັນສະມາຊິກ ຂອງບໍລິສັດລາວ ເນເຊີນນໍເພເມັ້ນ ເນັດເວີກ.",
    duration: "",
    img: "/product/allpd/product1.jpg",
    g1: "#0047ff",
    g2: "#120a2b",
    link: "/products_service/atm-inquiry",
  },
  {
    id: 2,
    title: "ຖອນເງິນສົດຂ້າມທະນາຄານຜ່ານຕູ້ ATM",
    desc: "ການຖອນເງິນສົດຜ່ານຕູ້ເອທີເອັມ ແມ່ນນຳໃຊ້ລະບົບແລັບໃນການ ເຊື່ອມໂຍງຕູ້ເອທີເອັມຂອງແຕ່ລະທະນາຄານທີ່ເປັນສະມາຊິກຂອງ ບໍລິສັດລາວ ເນເຊີນນໍເພເມັ້ນ ເນັດເວີກ.",
    duration: "",
    img: "/product/allpd/product2.jpg",
    g1: "#0047ff",
    g2: "#120a2b",
    link: "/products_service/atm-cash-withdraw",
  },
  {
    id: 3,
    title: "ໂອນເງິນຂ້າມທະນາຄານຜ່ານຕູ້ ATM",
    desc: "ການໂອນເງິນຂ້າມທະນາຄານຜ່ານຕູ້ ATM ແມ່ນນຳໃຊ້ລະບົບແລັບ ໃນການເຊື່ອມໂຍງຕູ້ເອທີເອັມຂອງແຕ່ລະທະນາຄານທີ່ເປັນສະມາຊິກຂອງ ບໍລິສັດລາວ ເນເຊີນນໍເພເມັ້ນ ເນັດເວີກ.",
    duration: "",
    img: "/product/allpd/product3.jpg",
    g1: "#0047ff",
    g2: "#120a2b",
    link: "/products_service/atm-transfer",
  },
  {
    id: 4,
    title: "ໂອນເງິນຂ້າມທະນາຄານເທິງມືຖື",
    desc: "ການໂອນເງິນຂ້າມທະນາຄານເທິງມືຖື (Fund Transfer via Mobile Application) ໂດຍນຳໃຊ້ລະບົບ LMPS ໃນການເຊື່ອມໂຍງຜ່ານ Applications ຂອງແຕ່ລະທະນາຄານທີ່ເປັນສະມາຊິກຂອງບໍລິສັດລາວ ເນເຊີນນໍເພເມັ້ນ ເນັດເວີກ",
    duration: "",
    img: "/product/allpd/product4.jpg",
    g1: "#0047ff",
    g2: "#120a2b",
    link: "/products_service/mobile-transfer",
  },
  {
    id: 5,
    title: "ຊໍາລະຂ້າມທະນາຄານຜ່ານ LAPNet",
    desc: "ການຊໍາລະຂ້າມທະນາຄານຜ່ານ LAPNet LAOQR ໂດຍນຳໃຊ້ລະບົບ LMPS ໃນການເຊື່ອມໂຍງຜ່ານ Applications ຂອງແຕ່ລະທະນາຄານທີ່ເປັນສະມາຊິກຂອງບໍລິສັດ LAPNet.",
    duration: "",
    img: "/product/allpd/product5.jpg",
    g1: "#0047ff",
    g2: "#120a2b",
    link: "/products_service/qr-payment",
  },
  {
    id: 6,
    title: "ຊຳລະຂ້າມແດນຜ່ານ QR CODE",
    desc: "ບໍລິສັດ LAPNet ໄດ້ຕໍ່ຍອດການພັດທະນາລະບົບຊຳລະຂ້າມທະນາຄານເທິງມືຖື (LMPS) ເພື່ອຂະຫຍາຍຜະລິດຕະພັນ ຊຳລະຂ້າມທະນາຄານ ໃຫ້ສາມາດຮອງຮັບການ ຊຳລະ ຄ່າສິນຄ້າ - ບໍລິການ ລະຫວ່າງປະເທດ.",
    duration: "",
    img: "/product/allpd/product6.jpg",
    g1: "#0047ff",
    g2: "#120a2b",
    link: "/products_service/crossborder",
  },
];

function getIndexFromQuery() {
  const id = Number(route.query.id);
  if (!Number.isFinite(id)) return 0;

  const idx = items.findIndex((it) => it.id === id);
  return idx >= 0 ? idx : 0;
}

const activeIndex = ref(getIndexFromQuery());
const activeItem = computed(() => items[activeIndex.value]);

function applyGradientInstant(item) {
  if (!heroEl.value) return;
  heroEl.value.style.setProperty("--g1", item.g1);
  heroEl.value.style.setProperty("--g2", item.g2);
}

// ✅ scroll ให้การ์ด active อยู่ "กลางจอ"
async function scrollActiveCardIntoView(behavior = "smooth") {
  await nextTick();

  // (1) ให้ section hero อยู่ใน viewport ก่อน
  if (heroEl.value?.scrollIntoView) {
    heroEl.value.scrollIntoView({ behavior, block: "start" });
  }

  // (2) แล้วค่อยเลื่อนให้ card ที่ active อยู่กลางจอ
  const el = cardRefs.value?.[activeIndex.value];
  if (el?.scrollIntoView) {
    // delay นิดนึงให้ layout stable ก่อน (กันกรณี GSAP/ภาพโหลด)
    requestAnimationFrame(() => {
      el.scrollIntoView({ behavior, block: "center", inline: "nearest" });
    });
  }
}

async function setActive(idx) {
  if (idx === activeIndex.value || isAnimating.value) return;
  isAnimating.value = true;

  const nextItem = items[idx];
  const reduceMotion =
    typeof window !== "undefined" &&
    window.matchMedia?.("(prefers-reduced-motion: reduce)")?.matches;

  if (reduceMotion) {
    activeIndex.value = idx;
    applyGradientInstant(nextItem);
    isAnimating.value = false;
    await scrollActiveCardIntoView("auto");
    return;
  }

  // animate OUT
  await Promise.all([
    imgEl.value
      ? gsap.to(imgEl.value, {
          opacity: 0,
          y: 14,
          scale: 0.985,
          duration: 0.22,
          ease: "power2.in",
        })
      : Promise.resolve(),
    titleEl.value
      ? gsap.to(titleEl.value, {
          opacity: 0,
          y: 10,
          duration: 0.18,
          ease: "power2.in",
        })
      : Promise.resolve(),
    subtitleEl.value
      ? gsap.to(subtitleEl.value, {
          opacity: 0,
          y: 10,
          duration: 0.18,
          ease: "power2.in",
        })
      : Promise.resolve(),
  ]);

  // swap
  activeIndex.value = idx;
  await nextTick();

  // gradient tween
  if (heroEl.value) {
    gsap.to(heroEl.value, {
      "--g1": nextItem.g1,
      "--g2": nextItem.g2,
      duration: 0.7,
      ease: "power2.out",
    });
  }

  // animate IN
  await Promise.all([
    titleEl.value
      ? gsap.fromTo(
          titleEl.value,
          { opacity: 0, y: -10 },
          { opacity: 1, y: 0, duration: 0.35, ease: "power3.out" }
        )
      : Promise.resolve(),
    subtitleEl.value
      ? gsap.fromTo(
          subtitleEl.value,
          { opacity: 0, y: -8 },
          { opacity: 1, y: 0, duration: 0.35, ease: "power3.out" }
        )
      : Promise.resolve(),
    imgEl.value
      ? gsap.fromTo(
          imgEl.value,
          { opacity: 0, y: -14, scale: 0.985 },
          { opacity: 1, y: 0, scale: 1, duration: 0.55, ease: "power3.out" }
        )
      : Promise.resolve(),
  ]);

  isAnimating.value = false;

  // ✅ หลังเปลี่ยน active แล้ว scroll ให้การ์ดอยู่กลางจอ
  await scrollActiveCardIntoView("smooth");
}

/* ✅ ถ้า query.id เปลี่ยน ให้เลือกอันใหม่ + scroll ให้การ์ดนั้นอยู่กลางจอ */
watch(
  () => route.query.id,
  async () => {
    const idx = getIndexFromQuery();
    if (idx !== activeIndex.value) {
      await setActive(idx); // setActive จะ scroll ให้เอง
    } else {
      // กรณี query เดิม แต่ต้องการให้เลื่อนมาอยู่กลางจอ (เช่นกลับมาหน้านี้)
      await scrollActiveCardIntoView("smooth");
    }
  }
);

onMounted(async () => {
  applyGradientInstant(activeItem.value);

  const reduceMotion =
    typeof window !== "undefined" &&
    window.matchMedia?.("(prefers-reduced-motion: reduce)")?.matches;

  // ✅ ตอนเข้าหน้าด้วย query id=2 (จาก product2) ให้เลื่อนมาที่ card นั้นทันที
  await scrollActiveCardIntoView(reduceMotion ? "auto" : "smooth");

  if (reduceMotion) return;

  const tl = gsap.timeline({ defaults: { ease: "power3.out" } });
  tl.from(".pill", { opacity: 0, y: -10, duration: 0.35 })
    .from(".title", { opacity: 0, y: 16, duration: 0.45 }, "-=0.1")
    .from(".subtitle", { opacity: 0, y: 10, duration: 0.4 }, "-=0.2")
    .from(".actions", { opacity: 0, y: 10, duration: 0.35 }, "-=0.25")
    .from(".media__frame", { opacity: 0, y: 16, duration: 0.5 }, "-=0.15")
    .from(".card", { opacity: 0, x: 18, duration: 0.38, stagger: 0.06 }, "-=0.25");
});
</script>

<style scoped>
/* ===== Background / Layout ===== */
.hero {
  --g1: #7b5cff;
  --g2: #070614;

  min-height: 100svh;
  display: grid;
  place-items: center;
  padding: clamp(18px, 4vw, 44px);

  background: radial-gradient(
      1200px 700px at 30% 0%,
      color-mix(in oklab, var(--g1) 70%, transparent) 0%,
      transparent 60%
    ),
    radial-gradient(
      900px 600px at 90% 40%,
      color-mix(in oklab, var(--g1) 35%, transparent) 0%,
      transparent 65%
    ),
    linear-gradient(
      180deg,
      color-mix(in oklab, var(--g1) 22%, #0a0620) 0%,
      var(--g2) 65%,
      #05040c 100%
    );
  overflow: hidden;
}

.hero__inner {
  width: min(1120px, 100%);
  display: grid;
  grid-template-columns: 1.1fr 0.9fr;
  gap: clamp(18px, 3vw, 28px);

  padding: clamp(18px, 3vw, 28px);
  border-radius: 28px;

  background: linear-gradient(180deg, rgba(0, 0, 0, 0.06), rgba(0, 0, 0, 0.02));
  border: 1px solid rgba(0, 0, 0, 0.1);
  box-shadow: 0 30px 80px rgba(0, 0, 0, 0.45), inset 0 1px 0 rgba(0, 0, 0, 0.08);

  backdrop-filter: blur(14px);
}

/* ===== Left ===== */
.left {
  display: grid;
  align-content: start;
  gap: 14px;
}

.pill {
  width: fit-content;
  padding: 10px 14px;
  border-radius: 999px;
  font-size: 14px;
  letter-spacing: 0.2px;
  color: rgba(255, 255, 255, 0.9);

  background: linear-gradient(180deg, rgba(255, 255, 255, 0.14), rgba(255, 255, 255, 0.06));
  border: 1px solid rgba(255, 255, 255, 0.16);
}

.title {
  margin: 0;
  font-weight: 650;
  line-height: 1.06;
  font-size: clamp(34px, 4vw, 54px);
  color: rgba(255, 255, 255, 0.96);
}

.subtitle {
  margin: 0;
  font-size: 15px;
  line-height: 1.5;
  color: rgba(255, 255, 255, 0.7);
}

.actions {
  margin-top: 2px;
  display: flex;
  gap: 12px;
}

.btn-read {
  position: relative;
  display: inline-flex;
  align-items: center;
  gap: 10px;
  padding: 12px 16px;
  border-radius: 14px;
  text-decoration: none;
  user-select: none;

  color: rgba(255, 255, 255, 0.92);
  background: linear-gradient(
    180deg,
    color-mix(in oklab, var(--g1) 18%, rgba(255, 255, 255, 0.12)),
    rgba(255, 255, 255, 0.05)
  );
  border: 1px solid color-mix(in oklab, var(--g1) 35%, rgba(255, 255, 255, 0.12));
  box-shadow: 0 18px 46px rgba(0, 0, 0, 0.35), inset 0 1px 0 rgba(255, 255, 255, 0.08);

  transition: transform 200ms ease, box-shadow 200ms ease, border-color 200ms ease;
  overflow: hidden;
}

.btn-read__text {
  font-size: 14px;
  font-weight: 650;
  letter-spacing: 0.2px;
}

.btn-read__icon {
  font-size: 16px;
  opacity: 0.9;
  transform: translateX(0);
  transition: transform 200ms ease, opacity 200ms ease;
}

.btn-read__shine {
  position: absolute;
  inset: -40%;
  background: radial-gradient(400px 160px at 20% 20%, rgba(255, 255, 255, 0.18), transparent 60%);
  opacity: 0.45;
  pointer-events: none;
  transform: translateX(-10%);
  transition: transform 260ms ease, opacity 260ms ease;
}

.btn-read:hover {
  transform: translateY(-1px);
  border-color: color-mix(in oklab, var(--g1) 55%, rgba(255, 255, 255, 0.14));
  box-shadow: 0 22px 60px rgba(0, 0, 0, 0.42), inset 0 1px 0 rgba(255, 255, 255, 0.1);
}

.btn-read:hover .btn-read__icon {
  transform: translateX(2px);
  opacity: 1;
}

.btn-read:hover .btn-read__shine {
  transform: translateX(6%);
  opacity: 0.6;
}

.btn-read:active {
  transform: translateY(0px);
}

.media {
  margin-top: 10px;
}

.media__frame {
  position: relative;
  overflow: hidden;
  border-radius: 22px;
  height: clamp(220px, 28vw, 340px);

  background: rgba(10, 8, 20, 0.55);
  border: 1px solid rgba(255, 255, 255, 0.1);

  box-shadow: 0 24px 60px rgba(0, 0, 0, 0.45), inset 0 1px 0 rgba(255, 255, 255, 0.08);
}

.media__img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  user-select: none;
  -webkit-user-drag: none;
  filter: saturate(1.05) contrast(1.02);
}

.media__glow {
  position: absolute;
  inset: -40%;
  background: radial-gradient(circle at 30% 20%, rgba(255, 255, 255, 0.18), transparent 55%);
  pointer-events: none;
}

/* ===== Right ===== */
.right {
  display: grid;
  gap: 14px;
  align-content: start;
}

.card {
  position: relative;
  text-align: left;
  border-radius: 18px;
  padding: 16px 16px 18px;

  background: linear-gradient(180deg, rgba(255, 255, 255, 0.08), rgba(255, 255, 255, 0.03));
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 16px 40px rgba(0, 0, 0, 0.35), inset 0 1px 0 rgba(255, 255, 255, 0.06);

  cursor: pointer;
  transition: transform 220ms ease, box-shadow 220ms ease, border-color 220ms ease;
}

.card:hover {
  transform: translateY(-2px);
  border-color: rgba(255, 255, 255, 0.18);
  box-shadow: 0 18px 52px rgba(0, 0, 0, 0.42), inset 0 1px 0 rgba(255, 255, 255, 0.08);
}

.card.is-active {
  border-color: color-mix(in oklab, var(--g1) 55%, rgba(255, 255, 255, 0.18));
  box-shadow: 0 20px 64px rgba(0, 0, 0, 0.46),
    0 0 0 1px rgba(255, 255, 255, 0.06) inset,
    0 0 40px color-mix(in oklab, var(--g1) 35%, transparent);
}

.card__top {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  margin-bottom: 10px;
}

.dots {
  display: flex;
  gap: 7px;
  opacity: 0.9;
}

.dot {
  width: 10px;
  height: 10px;
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.22);
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.12);
  transition: background 220ms ease, transform 220ms ease, box-shadow 220ms ease;
}

.card.is-active .dot.dot--on {
  background: color-mix(in oklab, var(--g1) 70%, rgba(255, 255, 255, 0.25));
  box-shadow: 0 0 18px color-mix(in oklab, var(--g1) 35%, transparent),
    inset 0 1px 0 rgba(255, 255, 255, 0.16);
  transform: scale(1.06);
}

.duration {
  font-size: 13px;
  color: rgba(255, 255, 255, 0.65);
}

.card__title {
  font-size: 18px;
  font-weight: 650;
  color: rgba(255, 255, 255, 0.92);
  margin-bottom: 6px;
}

.card__desc {
  font-size: 13px;
  line-height: 1.45;
  color: rgba(255, 255, 255, 0.68);
}

.card__border {
  position: absolute;
  inset: 0;
  border-radius: 18px;
  pointer-events: none;
  background: radial-gradient(
    400px 120px at 10% 10%,
    color-mix(in oklab, var(--g1) 30%, transparent),
    transparent 60%
  );
  opacity: 0;
  transition: opacity 220ms ease;
}

.card.is-active .card__border {
  opacity: 1;
}

/* ===== Responsive ===== */
@media (max-width: 900px) {
  .hero__inner {
    grid-template-columns: 1fr;
  }
  .media__frame {
    height: 260px;
  }
}
</style>
