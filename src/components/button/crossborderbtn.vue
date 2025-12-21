<template>
  <div class="lang-container">
    <div class="lang-box">
      <!-- LEFT -->
      <div
        class="select-wrap"
        ref="leftBlock"
        :class="{ open: open1 && !leftIsLa, 'is-la': leftIsLa }"
      >
        <div class="selected" :class="{ readonly: leftIsLa }" @click="toggleLeft">
          <img :src="getFlag(source)" class="flag" />
          <span>{{ getName(source) }}</span>
        </div>

        <!-- dropdown (เฉพาะฝั่งที่ไม่ใช่ la) -->
        <ul v-if="open1 && !leftIsLa" class="dropdown-list">
          <li
            v-for="lang in nonLaLanguages"
            :key="lang.code"
            @click="selectSource(lang.code)"
          >
            <img :src="lang.flag" class="flag" />
            <span>{{ lang.name }}</span>
          </li>
        </ul>
      </div>

      <!-- Swap -->
      <button
        @click="swap"
        class="swap-btn"
        ref="swapBtn"
        :class="{ 'swap-disabled': isSwapLocked }"
        :disabled="isSwapLocked"
      >
        <i class="fa-solid fa-repeat"></i>
      </button>

      <!-- RIGHT -->
      <div
        class="select-wrap"
        ref="rightBlock"
        :class="{ open: open2 && !rightIsLa, 'is-la': rightIsLa }"
      >
        <div
          class="selected"
          :class="{ readonly: rightIsLa }"
          @click="toggleRight"
        >
          <img :src="getFlag(target)" class="flag" />
          <span>{{ getName(target) }}</span>
        </div>

        <!-- dropdown (เฉพาะฝั่งที่ไม่ใช่ la) -->
        <ul v-if="open2 && !rightIsLa" class="dropdown-list">
          <li
            v-for="lang in nonLaLanguages"
            :key="lang.code"
            @click="selectTarget(lang.code)"
          >
            <img :src="lang.flag" class="flag" />
            <span>{{ lang.name }}</span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, watch, computed } from "vue"
import { useRouter } from "vue-router"
import { gsap } from "gsap"

const router = useRouter()

const languages = [
  { code: "la", name: "ລາວ", flag: "https://flagcdn.com/la.svg" },
  { code: "kh", name: "ກຳປູເຈຍ", flag: "https://flagcdn.com/kh.svg" },
  { code: "th", name: "ໄທ", flag: "https://flagcdn.com/th.svg" },
  { code: "vn", name: "ຫວຽດນາມ", flag: "https://flagcdn.com/vn.svg" },
  { code: "ch", name: "ຈີນ", flag: "https://flagcdn.com/cn.svg" },
]

// default initial pair
const source = ref("kh")
const target = ref("la")

const open1 = ref(false)
const open2 = ref(false)

const leftBlock = ref(null)
const rightBlock = ref(null)
const swapBtn = ref(null)

const isValidCode = (code) => languages.some((l) => l.code === code)
const getFlag = (code) => languages.find((l) => l.code === code)?.flag || ""
const getName = (code) => languages.find((l) => l.code === code)?.name || ""

const nonLaLanguages = computed(() => languages.filter((l) => l.code !== "la"))

const leftIsLa = computed(() => source.value === "la")
const rightIsLa = computed(() => target.value === "la")

// special case: vn-la and ch-la → swap is disabled
const isSwapLocked = computed(() => {
  return (
    target.value === "la" &&
    (source.value === "vn" || source.value === "ch")
  )
})

const toggleLeft = () => {
  if (leftIsLa.value) return
  open1.value = !open1.value
  if (open1.value) open2.value = false
}

const toggleRight = () => {
  if (rightIsLa.value) return
  open2.value = !open2.value
  if (open2.value) open1.value = false
}

// select from LEFT (non-la only)
const selectSource = (code) => {
  if (!isValidCode(code)) return
  if (code === "la") return

  source.value = code
  target.value = "la"
  open1.value = false
}

// select from RIGHT (non-la only)
const selectTarget = (code) => {
  if (!isValidCode(code)) return
  if (code === "la") return

  target.value = code
  source.value = "la"
  open2.value = false
}

// update route based on current pair
const updateRoute = () => {
  const path = `/products_service/crossborder/${source.value}-${target.value}`
  if (router.currentRoute.value.path === path) return

  router.push(path).catch((err) => {
    if (err && err.name !== "NavigationDuplicated") console.error(err)
  })
}

// sync state from route path (enforce rules: must include la, no la-la, etc.)
const syncStateFromRoute = (path) => {
  const match = path.match(/\/crossborder\/([a-z]+)-([a-z]+)/)
  if (!match) {
    source.value = "kh"
    target.value = "la"
    return
  }

  let src = match[1]
  let tgt = match[2]

  // normalize special cases:
  // la-vn  → vn-la
  // la-ch  → ch-la
  if (src === "la" && (tgt === "vn" || tgt === "ch")) {
    src = tgt
    tgt = "la"
  }

  if (!isValidCode(src) || !isValidCode(tgt)) {
    source.value = "kh"
    target.value = "la"
    return
  }

  // la-la → force only one side to be "la"
  if (src === "la" && tgt === "la") {
    src = "la"
    tgt = languages.find((l) => l.code !== "la")?.code || "kh"
  }
  // both sides are not "la" → force right side to "la"
  else if (src !== "la" && tgt !== "la") {
    tgt = "la"
  }

  source.value = src
  target.value = tgt
}

// redirect special cases: la-vn → vn-la, la-ch → ch-la (รองรับ path ที่มี prefix)
const fixWrongOrder = (path) => {
  const match = path.match(/\/crossborder\/([a-z]+)-([a-z]+)/)
  if (!match) return false

  const clean = `${match[1]}-${match[2]}`
  if (clean === "la-vn") {
    router.replace("/products_service/crossborder/vn-la")
    return true
  }
  if (clean === "la-ch") {
    router.replace("/products_service/crossborder/ch-la")
    return true
  }
  return false
}

// watch path: handle redirect + sync state
watch(
  () => router.currentRoute.value.path,
  (newPath, oldPath) => {
    if (newPath === oldPath) return

    const redirected = fixWrongOrder(newPath)
    if (redirected) return

    syncStateFromRoute(newPath)
  },
  { immediate: true }
)

// watch state → update route to match state
watch([source, target], () => {
  updateRoute()
})

// GSAP animated swap
const swap = () => {
  if (isSwapLocked.value) return

  open1.value = false
  open2.value = false

  const leftEl = leftBlock.value
  const rightEl = rightBlock.value
  const btnEl = swapBtn.value

  const doSwapLogic = () => {
    let src = source.value
    let tgt = target.value

    ;[src, tgt] = [tgt, src]

    // enforce rule: one side must be "la" and no la-la
    if (src !== "la" && tgt !== "la") tgt = "la"
    if (src === "la" && tgt === "la") {
      tgt = languages.find((l) => l.code !== "la")?.code || "kh"
    }

    source.value = src
    target.value = tgt
  }

  if (!leftEl || !rightEl || !btnEl) {
    doSwapLogic()
    return
  }

  const tl = gsap.timeline()

  tl.to(btnEl, {
    rotation: "+=180",
    duration: 0.25,
    ease: "power2.inOut",
  })

  tl.to(
    leftEl,
    {
      x: 40,
      scale: 0.97,
      opacity: 0.85,
      duration: 0.18,
      ease: "power2.in",
    },
    "<"
  )

  tl.to(
    rightEl,
    {
      x: -40,
      scale: 0.97,
      opacity: 0.85,
      duration: 0.18,
      ease: "power2.in",
    },
    "<"
  )

  tl.add(() => doSwapLogic())

  tl.to(leftEl, {
    x: 0,
    scale: 1,
    opacity: 1,
    duration: 0.22,
    ease: "back.out(1.6)",
  })

  tl.to(
    rightEl,
    {
      x: 0,
      scale: 1,
      opacity: 1,
      duration: 0.22,
      ease: "back.out(1.6)",
    },
    "<"
  )
}
</script>

<style scoped>
/* ====== Responsive wrapper ====== */
.lang-container {
  width: 100%;
  margin: 0 auto;
  padding: clamp(12px, 3vw, 24px);
  display: flex;
  justify-content: center;
  box-sizing: border-box;
}

/* ====== card ====== */
.lang-box {
  width: min(100%, 820px);
  background: linear-gradient(135deg, #001072, #5ab3dd);
  padding: clamp(14px, 2.2vw, 26px) clamp(14px, 2.6vw, 34px);
  border-radius: 22px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 14px;
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.35);
  z-index: 3;

  flex-wrap: wrap;
  box-sizing: border-box;

  /* สำคัญ: ทำให้เป็น stacking context */
  position: relative;
}

/* ====== Select blocks ====== */
.select-wrap {
  position: relative;
  flex: 1 1 260px;
  min-width: 220px;
  max-width: 420px;

  /* base layer */
  z-index: 1;
}

/* ตอนเปิด dropdown ให้ลอยขึ้นเหนือทุกอย่าง */
.select-wrap.open {
  z-index: 200;
}

/* selected input */
.selected {
  width: 100%;
  background: rgba(0, 0, 0, 0.35);
  color: #fff;
  padding: 12px 14px;
  border-radius: 15px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 10px;
  cursor: pointer;
  backdrop-filter: blur(4px);
  box-sizing: border-box;
}

.selected.readonly {
  cursor: default;
  opacity: 0.85;
}

/* flag */
.flag {
  width: clamp(20px, 2.4vw, 26px);
  height: clamp(14px, 1.7vw, 18px);
  border-radius: 4px;
  object-fit: cover;
}

.selected > span {
  flex: 1 1 auto;
  font-size: clamp(0.8rem, 1.2vw, 0.95rem);
  line-height: 1.2;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

/* dropdown */
.dropdown-list {
  position: absolute;
  left: 0;
  top: calc(100% + 8px);
  width: 100%;
  background: #0e0e0e;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.35);

  /* สำคัญ: ให้ทับทุกอย่างแน่นอน */
  z-index: 9999;

  max-height: min(320px, 40vh);
  overflow-y: auto;
}

/* dropdown items */
.dropdown-list li {
  padding: 11px 14px;
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  color: #fff;
  font-size: clamp(0.8rem, 1.2vw, 0.95rem);
}
.dropdown-list li:hover {
  background: rgba(255, 255, 255, 0.1);
}

/* ====== Swap button ====== */
.swap-btn {
  background: rgba(0, 0, 0, 0.35);
  color: #fff;
  border: none;
  width: 48px;
  height: 48px;
  border-radius: 14px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transform-origin: center;
  flex: 0 0 auto;
  touch-action: manipulation;

  /* ให้ปุ่มไม่โดน dropdown ทับผิดๆ ตอนมือถือ */
  position: relative;
  z-index: 2;
}

.swap-btn i {
  font-size: 16px;
}

.swap-disabled {
  opacity: 0.4;
  cursor: not-allowed;
  pointer-events: none;
}

/* ====== Tablet ====== */
@media (max-width: 768px) {
  .lang-box {
    gap: 12px;
    border-radius: 18px;
  }

  .select-wrap {
    flex: 1 1 320px;
    min-width: 200px;
  }
}

/* ====== Mobile ====== */
@media (max-width: 480px) {
  .lang-box {
    align-items: stretch;
    gap: 10px;
    padding: 14px;
  }

  .select-wrap {
    width: 100%;
    min-width: 0;
    max-width: none;
  }

  /* ✅ ตามที่ขอ: ถ้าฝั่งนั้นเป็น "ลาว" ให้ z-index ติดลบ เพื่อไม่ไปซ้อน dropdown */
  .select-wrap.is-la {
    z-index: -999;
  }

  /* ถ้าฝั่งนั้นกำลังเปิด dropdown ให้ดึงขึ้นมาเหนือเสมอ */
  .select-wrap.open {
    z-index: 200;
  }

  .swap-btn {
    width: 44px;
    height: 44px;
    border-radius: 12px;
    align-self: center;
    z-index: 2;
  }

  .dropdown-list {
    max-height: 45vh;
  }
}
</style>
