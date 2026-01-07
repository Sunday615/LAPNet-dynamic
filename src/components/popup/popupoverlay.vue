<template>
  <teleport to="body">
    <div
      v-if="isOpen"
      ref="overlayEl"
      class="po-overlay"
      role="dialog"
      aria-modal="true"
      aria-label="Announcement overlay popup"
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
          <div class="po-media" ref="mediaEl" :style="{ aspectRatio: imgAspect }">
            <img
              v-if="imageSrc"
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
            <h2 class="po-title">{{ titleText }}</h2>
            <p class="po-desc">{{ descText }}</p>

            <div class="po-actions" ref="actionsEl">
              <button v-if="hasLink" class="po-btn po-primary" type="button" @click="onPrimary">
                {{ primaryText }}
              </button>

              <button class="po-btn po-ghost" type="button" @click="close">
                ປິດ
              </button>
            </div>

            <!-- (optional debug) แสดง timeforshow -->
            <!-- <small style="opacity:.7">timeforshow: {{ current?.timeforshow }} ชั่วโมง</small> -->
          </div>
        </div>
      </div>
    </div>
  </teleport>
</template>

<script setup>
import { ref, computed, nextTick, onMounted, onBeforeUnmount } from "vue"
import { gsap } from "gsap"

const props = defineProps({
  // ✅ API
  apiUrl: { type: String, default: "http://localhost:3000/api/announcement" },

  // ✅ fallback
  fallbackTitle: { type: String, default: "Announcement" },
  fallbackDescription: { type: String, default: "" },
  fallbackImageAlt: { type: String, default: "Announcement image" },

  // ✅ auto popup
  autoShow: { type: Boolean, default: true },
  //test Func
    // debugAlwaysShow: { type: Boolean, default: false },
})

const emit = defineEmits(["primary", "closed", "loaded", "error"])

// refs (DOM)
const overlayEl = ref(null)
const cardEl = ref(null)
const closeBtnEl = ref(null)
const mediaEl = ref(null)
const imgEl = ref(null)
const bodyEl = ref(null)
const actionsEl = ref(null)
const gridEl = ref(null)

// state
const isOpen = ref(false)
const announcements = ref([]) // rows from API (ทั้งหมด)
const queue = ref([]) // รายการที่ "ถึงเวลาโชว์" แล้ว
const queueIndex = ref(0)
const current = computed(() => queue.value[queueIndex.value] || null)
const imgAspect = ref("16 / 9")

// timer
let nextCheckTimer = null

// animations
let introTl = null
let outroTl = null
let floatTween = null
let gridTween = null
let isClosing = false

// ==============================
// ✅ mapping from API table
// image-src -> image_url (ถ้าไม่มีใช้ image)
// title -> title
// description -> description
// primary button -> linkpath
// secondary button -> close
// timeforshow -> ชั่วโมง/1ครั้ง (3 = 3 ชั่วโมง)
// ==============================
const imageSrc = computed(() => {
  const a = current.value
  return a?.image_url || a?.image || ""
})

const imageAlt = computed(() => props.fallbackImageAlt)

const titleText = computed(() => {
  const a = current.value
  return (a?.title || "").trim() || props.fallbackTitle
})

const descText = computed(() => {
  const a = current.value
  return (a?.description || "").trim() || props.fallbackDescription
})

const hasLink = computed(() => {
  const link = String(current.value?.linkpath || "").trim()
  return !!link
})

const primaryText = computed(() => {
  const link = String(current.value?.linkpath || "").trim()
  if (!link) return ""
  try {
    const u = new URL(link)
    return `ອ່ານເພີ່ມ`
  } catch {
    return "ປິດ"
  }
})

function onImgLoad(e) {
  const img = e?.target
  if (img?.naturalWidth && img?.naturalHeight) {
    imgAspect.value = `${img.naturalWidth} / ${img.naturalHeight}`
  }
}

// ==============================
// ✅ timeforshow (ชั่วโมง) -> interval ms
// 3 = 3 ชั่วโมง / 1 ครั้ง
// 7 = 7 ชั่วโมง / 1 ครั้ง
// ==============================
function getIntervalMsFor(row) {
  // ✅ อ่าน timeforshow จาก API และตีเป็น "ชั่วโมง"
  const raw = row?.timeforshow
  const n = Number(raw)

  // ถ้าไม่มี/ไม่ถูกต้อง -> default 3 ชั่วโมง
  const hours = Number.isFinite(n) && n > 0 ? n : 3
  return hours * 60 * 60 * 1000
}

function getStorageKeyFor(row) {
  const id = row?.idannouncement
  return `announcement_last_shown_${id ?? "unknown"}`
}

function getLastShownMsFor(row) {
  try {
    const v = Number(localStorage.getItem(getStorageKeyFor(row)))
    return Number.isFinite(v) ? v : 0
  } catch {
    return 0
  }
}

function setLastShownMsFor(row, ms) {
  try {
    localStorage.setItem(getStorageKeyFor(row), String(ms))
  } catch {}
}

function isRowActive(row) {
  return Number(row?.active) === 1
}
//test showfunc
// function isDueToShow(row) {
//   if (!row) return false
//   if (!isRowActive(row)) return false

//   // ✅ โหมดเทส: แสดงทันที ไม่สน timeforshow/localStorage
//   if (props.debugAlwaysShow) return true

//   const interval = getIntervalMsFor(row)
//   const last = getLastShownMsFor(row)
//   const now = Date.now()
//   return !last || now - last >= interval
// }

function isDueToShow(row) {
  if (!row) return false
  if (!isRowActive(row)) return false

  const interval = getIntervalMsFor(row)
  const last = getLastShownMsFor(row)
  const now = Date.now()

  return !last || now - last >= interval
}

// สร้างคิว: เอาทุก announcement ที่ถึงเวลาแล้ว -> โชว์ทีละอัน (pop1 ปิด -> pop2 ต่อ)
function buildQueueFromRows(rows) {
  const arr = Array.isArray(rows) ? rows : []
  // ✅ เรียงให้แน่นอน (ใหม่ -> เก่า)
  arr.sort((a, b) => Number(b?.idannouncement ?? 0) - Number(a?.idannouncement ?? 0))
  return arr.filter(isDueToShow)
}

// หาเวลาที่จะถึงรอบถัดไป (เพื่อ setTimeout)
function computeNextRemainingMs(rows) {
  const now = Date.now()
  let min = Infinity
  let hasActive = false

  for (const row of rows || []) {
    if (!isRowActive(row)) continue
    hasActive = true

    const interval = getIntervalMsFor(row)
    const last = getLastShownMsFor(row)

    if (!last) return 0 // ยังไม่เคยโชว์ -> ได้เลย

    const elapsed = now - last
    const remaining = interval - elapsed
    if (remaining <= 0) return 0
    if (remaining < min) min = remaining
  }

  if (!hasActive) return null
  return min === Infinity ? null : min
}

function scheduleNextCheck() {
  if (!props.autoShow) return
  clearTimeout(nextCheckTimer)

  const remaining = computeNextRemainingMs(announcements.value)
  if (remaining === null) return

  // ✅ ไม่ต้องรอขั้นต่ำ 5 วิ ก็ได้ แต่กัน spam ด้วยขั้นต่ำ 1 วิ
  const wait = Math.max(1_000, remaining)

  nextCheckTimer = setTimeout(() => {
    fetchAnnouncements()
  }, wait)
}

// ==============================
// ✅ fetch announcements (ทั้งหมด)
// ==============================
async function fetchAnnouncements() {
  try {
    const res = await fetch(props.apiUrl, { method: "GET" })
    if (!res.ok) throw new Error(`Fetch failed: ${res.status}`)
    const json = await res.json()

    const rows = Array.isArray(json?.data) ? json.data : []
    announcements.value = rows
    emit("loaded", rows)

    await nextTick()
    tryAutoOpenQueue()
  } catch (e) {
    console.error("ANNOUNCEMENT FETCH ERROR:", e)
    emit("error", e)
  }
}

// ==============================
// ✅ open queue logic
// ==============================
async function tryAutoOpenQueue() {
  if (!props.autoShow) return
  if (isOpen.value) return

  const q = buildQueueFromRows(announcements.value)
  queue.value = q
  queueIndex.value = 0

  if (!queue.value.length) {
    scheduleNextCheck()
    return
  }

  // ✅ mark shown ให้ "ทุกตัวที่อยู่ในคิวรอบนี้" เพื่อให้ 1 รอบ/ตามชั่วโมง
  // ป้องกัน close แล้วย้อนกลับมาเปิดซ้ำทันทีตัวเดิมในรอบเดียว
  const now = Date.now()
  for (const row of queue.value) setLastShownMsFor(row, now)

  open()
}

// ==============================
// ✅ animations open/close/next
// ==============================
function onKeydown(e) {
  if (!isOpen.value) return
  if (e.key === "Escape") close()
}

async function playIntroFull() {
  await nextTick()

  introTl?.kill()
  outroTl?.kill()
  floatTween?.kill()
  gridTween?.kill()

  gsap.set(overlayEl.value, { opacity: 0 })
  gsap.set(cardEl.value, { opacity: 0, y: 46, scale: 0.985, rotateX: 6, transformPerspective: 900 })
  gsap.set([mediaEl.value, bodyEl.value, actionsEl.value], { opacity: 0, y: 14 })

  if (imgEl.value) gsap.set(imgEl.value, { scale: 1.12, transformOrigin: "50% 50%" })

  introTl = gsap.timeline({ defaults: { ease: "power3.out" } })
  introTl
    .to(overlayEl.value, { opacity: 1, duration: 0.28 })
    .to(cardEl.value, { opacity: 1, y: 0, rotateX: 0, scale: 1, duration: 0.55 }, "-=0.08")
    .to(mediaEl.value, { opacity: 1, y: 0, duration: 0.33 }, "-=0.25")
    .to(bodyEl.value, { opacity: 1, y: 0, duration: 0.33 }, "-=0.28")
    .to(actionsEl.value, { opacity: 1, y: 0, duration: 0.26 }, "-=0.22")

  if (imgEl.value) introTl.to(imgEl.value, { scale: 1, duration: 0.65, ease: "power2.out" }, "-=0.55")

  floatTween = gsap.to(cardEl.value, { y: "+=8", duration: 3.2, ease: "sine.inOut", yoyo: true, repeat: -1 })
  gridTween = gsap.to(gridEl.value, { x: "-=40", y: "+=30", duration: 8, ease: "sine.inOut", yoyo: true, repeat: -1 })

  closeBtnEl.value?.focus?.()
}

async function playSwapToNext() {
  introTl?.kill()
  floatTween?.kill()
  gridTween?.kill()

  const tl = gsap.timeline({ defaults: { ease: "power2.inOut" } })
  tl.to([actionsEl.value, bodyEl.value, mediaEl.value], { opacity: 0, y: 10, duration: 0.16, stagger: 0.03 })
  await new Promise((resolve) => tl.eventCallback("onComplete", resolve))

  queueIndex.value += 1
  imgAspect.value = "16 / 9"
  await nextTick()

  gsap.set([mediaEl.value, bodyEl.value, actionsEl.value], { opacity: 0, y: 14 })
  if (imgEl.value) gsap.set(imgEl.value, { scale: 1.12, transformOrigin: "50% 50%" })

  const tlIn = gsap.timeline({ defaults: { ease: "power3.out" } })
  tlIn
    .to(mediaEl.value, { opacity: 1, y: 0, duration: 0.28 })
    .to(bodyEl.value, { opacity: 1, y: 0, duration: 0.28 }, "-=0.22")
    .to(actionsEl.value, { opacity: 1, y: 0, duration: 0.22 }, "-=0.20")

  if (imgEl.value) tlIn.to(imgEl.value, { scale: 1, duration: 0.55, ease: "power2.out" }, "-=0.45")

  floatTween = gsap.to(cardEl.value, { y: "+=8", duration: 3.2, ease: "sine.inOut", yoyo: true, repeat: -1 })
  gridTween = gsap.to(gridEl.value, { x: "-=40", y: "+=30", duration: 8, ease: "sine.inOut", yoyo: true, repeat: -1 })

  closeBtnEl.value?.focus?.()
}

function open() {
  if (isOpen.value) return
  isOpen.value = true
  window.addEventListener("keydown", onKeydown)
  playIntroFull()
}

function close() {
  if (!isOpen.value || isClosing) return

  const hasNext = queueIndex.value + 1 < queue.value.length
  if (hasNext) {
    // ✅ ปิด popup 1 -> โชว์ popup 2 ต่อทันที (ไม่มี delay เพิ่มจาก timeforshow)
    playSwapToNext()
    return
  }

  isClosing = true
  introTl?.kill()
  floatTween?.kill()
  gridTween?.kill()

  outroTl?.kill()
  outroTl = gsap.timeline({
    defaults: { ease: "power2.inOut" },
    onComplete: () => {
      isOpen.value = false
      emit("closed")
      isClosing = false
      window.removeEventListener("keydown", onKeydown)

      queue.value = []
      queueIndex.value = 0

      // ✅ หลังปิดทั้งหมดแล้ว ค่อยตั้งเวลาเช็คตาม timeforshow (หน่วยชั่วโมง)
      scheduleNextCheck()
    },
  })

  outroTl
    .to([actionsEl.value, bodyEl.value, mediaEl.value], { opacity: 0, y: 10, duration: 0.18, stagger: 0.04 })
    .to(cardEl.value, { opacity: 0, y: 30, scale: 0.985, duration: 0.22 }, "-=0.08")
    .to(overlayEl.value, { opacity: 0, duration: 0.20 }, "-=0.10")
}

function onPrimary() {
  const link = String(current.value?.linkpath || "").trim()
  if (!link) return

  emit("primary", link)
  window.open(link, "_blank", "noopener,noreferrer")
  close()
}

onMounted(() => {
  fetchAnnouncements()
})

onBeforeUnmount(() => {
  window.removeEventListener("keydown", onKeydown)
  clearTimeout(nextCheckTimer)
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
.po-media{
  padding:14px;
  width:100%;
  min-height: 260px;
}
.po-img{
  width:100%;
  height:100%;
  border-radius:18px;
  object-fit: cover;
  object-position: center;
  border:1px solid rgba(96,165,250,0.18);
  box-shadow: 0 22px 70px rgba(0,0,0,0.45), inset 0 1px 0 rgba(255,255,255,0.06);
  transform: translateZ(0);
}
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
