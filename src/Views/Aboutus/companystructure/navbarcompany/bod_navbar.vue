<template>
  <nav class="tabs-nav">
    <ul class="tabs-list" ref="tabsListEl" @scroll.passive="onTabsScroll">
      <li
        v-for="tab in tabs"
        :key="tab.key"
        :ref="(el) => setTabRef(tab.key, el)"
        :class="['tab-item', { 'is-active': tab.key === activeTab }]"
        @click="selectTab(tab.key)"
      >
        {{ tab.label }}
      </li>
    </ul>
  </nav>
</template>

<script setup>
import { ref, watch, nextTick, onMounted, onBeforeUnmount } from 'vue'
import { useRouter, useRoute } from 'vue-router'

const router = useRouter()
const route = useRoute()

const tabs = [
  { key: 'board_of_director', label: 'ຄະນະສະພາຜູ້ບໍລິຫານ' },
  { key: 'boarddirector_audit', label: 'ຄະນະກຳມະການກວດກາ' },
  { key: 'policy', label: 'ຄະນະກຳມະການຄົ້ນຄວ້ານະໂຍບາຍ' },
  { key: 'risk', label: 'ຄະນະກຳມະການຄຸ້ມຄອງຄວາມສ່ຽງ' },
  { key: 'develop', label: 'ຄະນະກຳມະການຕິດຕາມພັດທະນາລະບົບ' },
]

const tabRouteMap = {
  board_of_director: '/aboutus/board_director',
  boarddirector_audit: '/board_of_director/audit_committee',
  policy: '/board_of_director/policy_research_committee',
  risk: '/board_of_director/riskmanagemen_committee',
  develop: '/board_of_director/develop_committee',
}

const normalizePath = (p) => (p || '').replace(/\/+$/, '')

// -------------------- Responsive-only: center active tab (mobile) --------------------
const tabsListEl = ref(null)
const tabRefs = ref({})

const setTabRef = (key, el) => {
  if (el) tabRefs.value[key] = el
}

const SCROLL_KEY = 'tabs-nav-scroll-left' // (optional) keep last scroll in case component remounts
const isMobile = ref(false)
let mq

const setupMediaQuery = () => {
  if (typeof window === 'undefined') return

  mq = window.matchMedia('(max-width: 768px)')
  isMobile.value = mq.matches

  const handler = (e) => {
    isMobile.value = e.matches
  }

  if (mq.addEventListener) mq.addEventListener('change', handler)
  else mq.addListener(handler)

  onBeforeUnmount(() => {
    if (mq?.removeEventListener) mq.removeEventListener('change', handler)
    else mq?.removeListener(handler)
  })
}

const saveScrollLeft = () => {
  if (!isMobile.value) return
  const el = tabsListEl.value
  if (!el) return
  sessionStorage.setItem(SCROLL_KEY, String(el.scrollLeft))
}

const restoreScrollLeft = async () => {
  if (!isMobile.value) return
  await nextTick()
  const el = tabsListEl.value
  if (!el) return

  const saved = sessionStorage.getItem(SCROLL_KEY)
  const n = saved == null ? NaN : Number(saved)
  if (!Number.isNaN(n)) el.scrollLeft = n
}

const centerActiveTab = async (key, behavior = 'smooth') => {
  if (!isMobile.value) return
  await nextTick()

  const container = tabsListEl.value
  const item = tabRefs.value[key]
  if (!container || !item) return

  const targetLeft =
    item.offsetLeft - (container.clientWidth / 2 - item.offsetWidth / 2)

  container.scrollTo({
    left: Math.max(0, targetLeft),
    behavior,
  })

  saveScrollLeft()
}

const onTabsScroll = () => {
  saveScrollLeft()
}

// -------------------- Active tab (fix selected) --------------------
const activeTab = ref(
  Object.keys(tabRouteMap).find(
    (key) => normalizePath(tabRouteMap[key]) === normalizePath(route.path)
  ) || tabs[0].key
)

const selectTab = async (key) => {
  activeTab.value = key

  // mobile: center the selected tab in frame
  await centerActiveTab(key, 'smooth')

  const path = tabRouteMap[key]
  if (path && normalizePath(path) !== normalizePath(route.path)) {
    try {
      await router.push(path)
    } catch (_) {}
  }
}

watch(
  () => route.path,
  async (newPath) => {
    const foundKey = Object.keys(tabRouteMap).find(
      (key) => normalizePath(tabRouteMap[key]) === normalizePath(newPath)
    )

    if (foundKey) {
      activeTab.value = foundKey

      // mobile: after route change (even if component remount), keep it centered
      await restoreScrollLeft()
      await centerActiveTab(foundKey, 'auto')
    }
  }
)

onMounted(async () => {
  setupMediaQuery()
  await restoreScrollLeft()
  await centerActiveTab(activeTab.value, 'auto')
})
</script>

<style scoped>
.tabs-nav {
  width: 90%;
  background: #f5f7fb;
  border-bottom: 1px solid rgba(15, 23, 42, 0.08);
  display: flex;
  justify-content: center;
}

.tabs-list {
  width: 100%;
  max-width: 1300px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 32px;
  padding: 0 24px;
  margin: 0;
  list-style: none;
  overflow-x: auto;
}

.tabs-list::-webkit-scrollbar {
  height: 3px;
}
.tabs-list::-webkit-scrollbar-thumb {
  background: rgba(148, 163, 184, 0.7);
  border-radius: 999px;
}

.tab-item {
  position: relative;
  padding: 14px 4px 12px;
  font-size: var(--fs-sm);
  font-weight: 500;
  color: #64748b;
  cursor: pointer;
  white-space: nowrap;
  transition: color 0.2s ease-out, transform 0.2s ease-out;
}

.tab-item::after {
  content: '';
  position: absolute;
  left: 0;
  bottom: 0;
  height: 3px;
  width: 100%;
  border-radius: 999px;
  background: linear-gradient(
    95deg,
    rgba(0, 238, 255, 1) 0%,
    rgba(0, 51, 171, 1) 76%
  );
  transform: scaleX(0);
  transform-origin: center;
  transition: transform 0.25s ease-out;
}

/* ✅ Hover effect ทำงานเฉพาะอุปกรณ์ที่ hover ได้จริง (Desktop/Mouse) */
@media (hover: hover) and (pointer: fine) {
  .tab-item:hover {
    color: #0f172a;
    transform: translateY(-1px);
  }

  .tab-item:hover::after {
    transform: scaleX(1);
  }
}

.tab-item.is-active {
  color: #0f172a;
  font-weight: 600;
}

.tab-item.is-active::after {
  transform: scaleX(1);
}

/* ✅ Responsive only */
@media (max-width: 768px) {
  .tabs-list {
    gap: 20px;
    padding-inline: 16px;
    justify-content: flex-start; /* ให้ scroll แนวนอนนิ่ง */
  }

  .tab-item {
    font-size: 14px;
    padding: 12px 2px 10px;
    transform: none; /* กันเด้งบนมือถือ */
  }
}
</style>
