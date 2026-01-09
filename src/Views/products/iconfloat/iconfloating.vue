<template>
  <section ref="root" class="fiHero">
    <!-- subtle light background -->
    <div class="bg" aria-hidden="true">
      <div class="bgRadial"></div>
      <div class="bgGrid"></div>
      <div class="bgNoise"></div>
    </div>

    <div class="wrap">
      <!-- LEFT: text + floating icons -->
      <div class="left">
        <div class="content">
          <p v-if="kicker" class="kicker">{{ kicker }}</p>

          <h1 class="title">
            <span>​ເງື່ອນໄຂ</span> {{ title }}
          </h1>

          <p class="desc">{{ description }}</p>
          <p class="desc">{{ descriptionsub }}</p>

          <div class="actions">
            <slot name="actions" />
          </div>
        </div>

        <!-- Floating icons layer -->
        <div ref="iconField" class="iconField" aria-hidden="true">
          <div
            v-for="(ic, i) in icons"
            :key="i"
            class="icon"
            :style="iconStyle(ic)"
            :ref="(el) => setIconRef(el, i)"
          >
            <img v-if="ic.src" :src="ic.src" :alt="ic.alt ?? ''" />
            <span v-else class="iconText">{{ ic.text ?? "✨" }}</span>
          </div>
        </div>
      </div>

      <!-- RIGHT -->
      <div class="right">
        <LogoFloating />
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount, nextTick, onBeforeUpdate } from "vue";
import type { ComponentPublicInstance } from "vue";
import LogoFloating from "../../../components/logofloating/logofloating.vue";

type FloatingIcon = {
  src?: string;
  alt?: string;
  text?: string;
  size?: number;
  x?: number;
  y?: number;
  opacity?: number;
  blur?: number;
};

/** ✅ IMPORTANT: no `const props =` here, so TS6133 is gone */
withDefaults(
  defineProps<{
    kicker?: string;
    title?: string;
    titleAccent?: string;
    description?: string;
    descriptionsub?: string;
    icons?: FloatingIcon[];
  }>(),
  {
    kicker: "",
    title: "ການໃຊ້ບໍລິການ",
    titleAccent: "",
    description:
      "ລຸກຄ້າສາມາດນຳໃຊ້ເລກບັນຊີທະນາຄານ ດຳເນີນທຸລະກຳເທິງມືຖື ໂດຍຜ່ານ Applications ຂອງແຕ່ລະທະນາຄານທຸລະກິດທີ່ເປັນສະມາຊິກຂອງບໍລິສັດ LAPNet ໄດ້ຟຣີໃນໄລຍະທີ່ເປີດຕົວຜະລິດຕະພັນການໂອນເງິນຂ້າມທະນາຄານເທິງມືຖືໃໝ່ ດັ່ງເນື້ອໃນລຸ່ມນີ້:",
    descriptionsub:
      "- ການໂອນເງິນເທີງມືຖືໂດຍນຳໃຊ້ຮູບແບບການໂອນ ບັນຊີຫາບັນຊີ ຫຼື ເອີ້ນວ່າ Account Fund Transfer ແມ່ນມີສະມາຊິກທັງໝົດ 18 ທະນາຄານ ແລະ 2 Fintech ດັ່ງນີ້:",
    icons: () => [],
  }
);

const root = ref<HTMLElement | null>(null);
const iconField = ref<HTMLElement | null>(null);

const iconRefs = ref<HTMLElement[]>([]);
onBeforeUpdate(() => (iconRefs.value = []));

function setIconRef(el: Element | ComponentPublicInstance | null, idx: number) {
  if (el instanceof HTMLElement) iconRefs.value[idx] = el;
}

function iconStyle(ic: FloatingIcon) {
  const size = ic.size ?? 52;
  return {
    left: `${ic.x ?? 50}%`,
    top: `${ic.y ?? 50}%`,
    width: `${size}px`,
    height: `${size}px`,
    opacity: `${ic.opacity ?? 0.9}`,
    filter: ic.blur ? `blur(${ic.blur}px)` : undefined,
  } as Record<string, string | undefined>;
}

let ctx: any = null;
let floatTweens: any[] = [];

function killFloats() {
  floatTweens.forEach((t) => t.kill());
  floatTweens = [];
}

onMounted(async () => {
  // SSR-safe
  if (typeof window === "undefined") return;

  // Load GSAP on client only
  const { gsap } = await import("gsap");

  await nextTick();

  const reduce =
    window.matchMedia?.("(prefers-reduced-motion: reduce)")?.matches ?? false;

  ctx = gsap.context(() => {
    gsap.from(".content > *", {
      y: 18,
      opacity: 0,
      duration: 0.85,
      ease: "power3.out",
      stagger: 0.08,
    });

    gsap.from(iconRefs.value, {
      scale: 0.9,
      opacity: 0,
      duration: 0.65,
      ease: "power3.out",
      stagger: 0.06,
      delay: 0.1,
    });

    if (!reduce) {
      killFloats();

      if (iconField.value) {
        floatTweens.push(
          gsap.to(iconField.value, {
            y: -6,
            duration: 3.8,
            ease: "sine.inOut",
            yoyo: true,
            repeat: -1,
          })
        );
      }

      iconRefs.value.forEach((el: HTMLElement, i: number) => {
        const amp = 10 + Math.random() * 18;
        const dur = 3.2 + Math.random() * 2.6;
        const rot = (Math.random() * 2 - 1) * 8;

        floatTweens.push(
          gsap.to(el, {
            x: `+=${(Math.random() * 2 - 1) * amp}`,
            y: `+=${(Math.random() * 2 - 1) * amp}`,
            rotation: rot,
            duration: dur,
            ease: "sine.inOut",
            yoyo: true,
            repeat: -1,
            delay: i * 0.08,
          })
        );
      });
    }
  }, root.value as any);
});

onBeforeUnmount(() => {
  killFloats();
  ctx?.revert?.();
  ctx = null;
});
</script>

<style scoped>
.title span {
  background: linear-gradient(120deg, #0033ab 0%, #0078ff 40%, #00c6ff 100%);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}

/* Light / white theme (neutral only) */
.fiHero {
  position: relative;
  padding: 64px 18px 72px;
  overflow: hidden;
  background: #ffffff;
  color: rgba(10, 10, 12, 0.92);
}

/* subtle background layers */
.bg {
  position: absolute;
  inset: 0;
  pointer-events: none;
  z-index: 0;
}
.bgRadial {
  position: absolute;
  inset: -25%;
  background: radial-gradient(
      900px 560px at 25% 10%,
      rgba(0, 0, 0, 0.05),
      transparent 60%
    ),
    radial-gradient(
      820px 620px at 70% 65%,
      rgba(0, 0, 0, 0.04),
      transparent 65%
    ),
    radial-gradient(
      980px 760px at 50% 95%,
      rgba(0, 0, 0, 0.035),
      transparent 65%
    );
  filter: blur(2px);
}
.bgGrid {
  position: absolute;
  inset: 0;
  background-image: linear-gradient(rgba(0, 0, 0, 0.05) 1px, transparent 1px),
    linear-gradient(90deg, rgba(0, 0, 0, 0.05) 1px, transparent 1px);
  background-size: 56px 56px;
  opacity: 0.2;
  mask-image: radial-gradient(
    closest-side at 35% 30%,
    rgba(0, 0, 0, 1),
    rgba(0, 0, 0, 0)
  );
}
.bgNoise {
  position: absolute;
  inset: 0;
  opacity: 0.06;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='180' height='180'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.8' numOctaves='2' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='180' height='180' filter='url(%23n)' opacity='.6'/%3E%3C/svg%3E");
  background-size: 180px 180px;
  mix-blend-mode: multiply;
}

/* layout */
.wrap {
  position: relative;
  z-index: 1;
  max-width: 100%;
  margin: 0 auto;
  display: grid;
  grid-template-columns: 0.7fr 1fr;
  gap: 18px;
  align-items: stretch;
}

/* left container */
.left {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: relative;
  border-radius: 26px;
  padding: 26px 22px;
  overflow: hidden;
  background: rgba(255, 255, 255, 0.78);
  border: 1px solid rgba(0, 0, 0, 0.08);
  backdrop-filter: blur(10px);
  box-shadow: 0 18px 60px rgba(0, 0, 0, 0.1),
    inset 0 1px 0 rgba(255, 255, 255, 0.8);
}

.content {
  position: relative;
  z-index: 2;
  max-width: 560px;
}

.kicker {
  margin: 0 0 10px;
  font-size: 12px;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: rgba(10, 10, 12, 0.6);
}

.title {
  margin: 0;
  font-size: clamp(32px, 4vw, 54px);
  line-height: 1.08;
  letter-spacing: -0.02em;
  font-weight: 800;
  color: rgba(10, 10, 12, 0.92);
}

.titleAccent {
  margin-left: 10px;
  font-weight: 900;
  color: rgba(10, 10, 12, 0.92);
  text-decoration: underline;
  text-decoration-thickness: 3px;
  text-underline-offset: 8px;
  opacity: 0.85;
}

.desc {
  margin: 20px 0 0;
  max-width: 560px;
  color: rgba(10, 10, 12, 0.68);
  font-size: 15px;
  line-height: 1.7;
}

.actions {
  margin-top: 16px;
}

/* floating icons */
.iconField {
  position: absolute;
  inset: 0;
  z-index: 1;
  pointer-events: none;
}

.icon {
  position: absolute;
  transform: translate(-50%, -50%);
  border-radius: 18px;
  display: grid;
  place-items: center;
  background: rgba(255, 255, 255, 0.92);
  border: 1px solid rgba(0, 0, 0, 0.08);
  box-shadow: 0 16px 40px rgba(0, 0, 0, 0.12);
  backdrop-filter: blur(8px);
}

.icon img {
  width: 70%;
  height: 70%;
  object-fit: contain;
}

.iconText {
  font-size: 22px;
}

/* right */
.right {
  border-radius: 26px;
  min-height: 320px;
  background: rgba(255, 255, 255, 0.7);
  border: 1px solid rgba(0, 0, 0, 0.08);
  backdrop-filter: blur(10px);
  box-shadow: 0 18px 60px rgba(0, 0, 0, 0.1),
    inset 0 1px 0 rgba(255, 255, 255, 0.8);
}

/* responsive */
@media (max-width: 980px) {
  .wrap {
    grid-template-columns: 1fr;
  }
  .right {
    min-height: 220px;
  }
}
</style>
