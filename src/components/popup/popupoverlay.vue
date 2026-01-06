<template>
  <teleport to="body">
    <div
      v-if="modelValue"
      ref="overlayEl"
      class="po-overlay"
      role="dialog"
      aria-modal="true"
      aria-label="Overlay popup"
      @click.self="close"
    >
      <div ref="cardEl" class="po-card">
        <button ref="closeBtnEl" class="po-close" type="button" aria-label="Close" @click="close">
          ✕
        </button>

        <!-- Dark-blue background -->
        <div class="po-bg" aria-hidden="true">
          <div ref="gridEl" class="po-grid"></div>
          <div class="po-glow po-glow-1"></div>
          <div class="po-glow po-glow-2"></div>
          <div class="po-noise"></div>
        </div>

        <div class="po-content">
          <!-- ✅ Media container follow image ratio -->
          <div class="po-media" ref="mediaEl" :style="{ aspectRatio: imgAspect }">
            <img
              ref="imgEl"
              class="po-img"
              :src="imageSrc"
              :alt="imageAlt"
              loading="eager"
              decoding="async"
              @load="onImgLoad"
            />
          </div>

          <div class="po-body" ref="bodyEl">
            <h2 class="po-title">{{ title }}</h2>
            <p class="po-desc">{{ description }}</p>

            <div class="po-actions" ref="actionsEl">
              <button class="po-btn po-primary" type="button" @click="onPrimary">
                {{ primaryText }}
              </button>
              <button class="po-btn po-ghost" type="button" @click="onSecondary">
                {{ secondaryText }}
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </teleport>
</template>

<script setup>
import { ref, watch, nextTick, onBeforeUnmount } from "vue"
import { gsap } from "gsap"

const props = defineProps({
  modelValue: { type: Boolean, default: false }, // v-model
  imageSrc: { type: String, required: true },
  imageAlt: { type: String, default: "Popup image" },
  title: { type: String, default: "Title" },
  description: { type: String, default: "Description" },
  primaryText: { type: String, default: "Primary" },
  secondaryText: { type: String, default: "Secondary" },
})

const emit = defineEmits(["update:modelValue", "primary", "secondary", "closed"])

const overlayEl = ref(null)
const cardEl = ref(null)
const closeBtnEl = ref(null)
const mediaEl = ref(null)
const imgEl = ref(null)
const bodyEl = ref(null)
const actionsEl = ref(null)
const gridEl = ref(null)


const imgAspect = ref("16 / 9")

function onImgLoad(e) {
  const img = e?.target
  if (img?.naturalWidth && img?.naturalHeight) {
    imgAspect.value = `${img.naturalWidth} / ${img.naturalHeight}`
  }
}

let introTl = null
let outroTl = null
let floatTween = null
let gridTween = null
let isClosing = false

function onKeydown(e) {
  if (!props.modelValue) return
  if (e.key === "Escape") close()
}

async function playIntro() {
  await nextTick()

  introTl?.kill()
  outroTl?.kill()
  floatTween?.kill()
  gridTween?.kill()

  gsap.set(overlayEl.value, { opacity: 0 })
  gsap.set(cardEl.value, { opacity: 0, y: 46, scale: 0.985, rotateX: 6, transformPerspective: 900 })
  gsap.set([mediaEl.value, bodyEl.value, actionsEl.value], { opacity: 0, y: 14 })

  // ✅ image zoom (container จะ “ตามสัดส่วน” ส่วนรูปจะ scale)
  gsap.set(imgEl.value, { scale: 1.12, transformOrigin: "50% 50%" })

  introTl = gsap.timeline({ defaults: { ease: "power3.out" } })
  introTl
    .to(overlayEl.value, { opacity: 1, duration: 0.28 })
    .to(cardEl.value, { opacity: 1, y: 0, rotateX: 0, scale: 1, duration: 0.55 }, "-=0.08")
    .to(mediaEl.value, { opacity: 1, y: 0, duration: 0.33 }, "-=0.25")
    .to(bodyEl.value, { opacity: 1, y: 0, duration: 0.33 }, "-=0.28")
    .to(actionsEl.value, { opacity: 1, y: 0, duration: 0.26 }, "-=0.22")
    .to(imgEl.value, { scale: 1, duration: 0.65, ease: "power2.out" }, "-=0.55")

  // “เลื้อยๆ” เบาๆ
  floatTween = gsap.to(cardEl.value, {
    y: "+=8",
    duration: 3.2,
    ease: "sine.inOut",
    yoyo: true,
    repeat: -1,
  })

  gridTween = gsap.to(gridEl.value, {
    x: "-=40",
    y: "+=30",
    duration: 8,
    ease: "sine.inOut",
    yoyo: true,
    repeat: -1,
  })

  closeBtnEl.value?.focus?.()
}

function close() {
  if (!props.modelValue || isClosing) return
  isClosing = true

  introTl?.kill()
  floatTween?.kill()
  gridTween?.kill()

  outroTl?.kill()
  outroTl = gsap.timeline({
    defaults: { ease: "power2.inOut" },
    onComplete: () => {
      emit("update:modelValue", false)
      emit("closed")
      isClosing = false
    },
  })

  outroTl
    .to([actionsEl.value, bodyEl.value, mediaEl.value], { opacity: 0, y: 10, duration: 0.18, stagger: 0.04 })
    .to(cardEl.value, { opacity: 0, y: 30, scale: 0.985, duration: 0.22 }, "-=0.08")
    .to(overlayEl.value, { opacity: 0, duration: 0.20 }, "-=0.10")
}

function onPrimary() {
  emit("primary")
  close()
}
function onSecondary() {
  emit("secondary")
  close()
}

watch(
  () => props.modelValue,
  (v) => {
    if (v) {
      window.addEventListener("keydown", onKeydown)
      playIntro()
    } else {
      window.removeEventListener("keydown", onKeydown)
    }
  },
  { immediate: true }
)

onBeforeUnmount(() => {
  window.removeEventListener("keydown", onKeydown)
  introTl?.kill()
  outroTl?.kill()
  floatTween?.kill()
  gridTween?.kill()
})
</script>

<style scoped>
.po-overlay{
  position:fixed; inset:0; z-index:9999;
  display:grid; place-items:center; padding:16px;
  background: rgba(1, 6, 18, 0.86);
  backdrop-filter: blur(14px);
}

/* ✅ เปลี่ยนจาก fixed height เป็น auto + max-height */
.po-card{
  position:relative;
  width:min(1100px, 92vw);
  max-height: 90vh;
  height: auto;
  border-radius:24px;
  overflow:hidden;
  background: linear-gradient(180deg, rgba(8,14,32,0.90), rgba(6,10,24,0.80));
  border:1px solid rgba(96,165,250,0.18);
  box-shadow: 0 40px 120px rgba(0,0,0,0.70), inset 0 1px 0 rgba(255,255,255,0.06);
  color: rgba(240,248,255,0.92);
}

.po-bg{position:absolute; inset:0; overflow:hidden;}
.po-grid{
  position:absolute; inset:-30%;
  background-image:
    linear-gradient(rgba(96,165,250,0.12) 1px, transparent 1px),
    linear-gradient(90deg, rgba(96,165,250,0.12) 1px, transparent 1px);
  background-size:60px 60px;
  transform: perspective(1000px) rotateX(58deg);
  opacity:0.18;
}
.po-glow{position:absolute; width:820px; height:820px; border-radius:50%; filter: blur(80px); opacity:0.55;}
.po-glow-1{left:-260px; top:-260px; background: radial-gradient(circle at 30% 30%, rgba(37,99,235,0.95), rgba(29,78,216,0) 70%);}
.po-glow-2{right:-260px; bottom:-260px; background: radial-gradient(circle at 40% 40%, rgba(56,189,248,0.85), rgba(37,99,235,0) 74%);}
.po-noise{
  position:absolute; inset:0; opacity:0.12; mix-blend-mode: overlay;
  background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='180' height='180'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.75' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='180' height='180' filter='url(%23n)' opacity='.55'/%3E%3C/svg%3E");
}

.po-content{
  position:relative; z-index:1;
  display:grid;
  grid-template-columns: 1.1fr 0.9fr;
  align-items: stretch;
}

/* ✅ container “ตามสัดส่วนรูป” ด้วย aspect-ratio */
.po-media{
  padding:14px;
  width:100%;
  min-height: 260px;
  /* aspect-ratio ถูกส่งจาก :style (imgAspect) */
}

.po-img{
  width:100%;
  height:100%;
  border-radius:18px;
  object-fit: cover;          /* ✅ ตามที่ต้องการ */
  object-position: center;    /* ✅ */
  border:1px solid rgba(96,165,250,0.18);
  box-shadow: 0 22px 70px rgba(0,0,0,0.45), inset 0 1px 0 rgba(255,255,255,0.06);
  transform: translateZ(0);
}

/* ✅ ถ้า content เยอะ ให้ body scroll แทนไม่ล้นจอ */
.po-body{
  padding:28px 26px 24px;
  display:flex;
  flex-direction:column;
  justify-content:center;
  overflow:auto;
  max-height: 90vh;
}

.po-title{
  margin:0 0 12px;
  font-size:clamp(22px, 3vw, 44px);
  line-height:1.05;
  letter-spacing:-0.7px;
}
.po-desc{
  margin:0 0 20px;
  color: rgba(219,234,254,0.78);
  line-height:1.65;
  max-width:48ch;
  font-size:15px;
}

.po-actions{display:flex; gap:12px; flex-wrap:wrap;}
.po-btn{
  border-radius:14px;
  padding:12px 16px;
  font-weight:800;
  border:1px solid rgba(96,165,250,0.20);
  color: rgba(240,248,255,0.94);
  cursor:pointer;
  transition: transform 180ms ease, background 180ms ease, border-color 180ms ease;
}
.po-primary{
  background: linear-gradient(135deg, rgba(37,99,235,1), rgba(56,189,248,0.95));
  border-color: rgba(255,255,255,0.10);
  box-shadow: 0 16px 46px rgba(37,99,235,0.22);
}
.po-primary:hover{transform: translateY(-1px);}
.po-ghost{background: rgba(37,99,235,0.10);}
.po-ghost:hover{transform: translateY(-1px); background: rgba(37,99,235,0.16); border-color: rgba(96,165,250,0.32);}

.po-close{
  position:absolute; top:14px; right:14px;
  width:46px; height:46px;
  border-radius:14px;
  border:1px solid rgba(96,165,250,0.18);
  background: rgba(0,0,0,0.32);
  color: rgba(240,248,255,0.92);
  cursor:pointer;
  z-index:2;
}

@media (max-width: 880px){
  .po-card{width:92vw; max-height:92vh;}
  .po-content{grid-template-columns:1fr;}
  .po-body{padding:16px; justify-content:flex-start;}
  .po-media{min-height: 220px;}
}
</style>
