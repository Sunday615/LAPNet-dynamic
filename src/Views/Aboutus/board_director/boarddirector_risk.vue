<template>
  <main_navbar
    title="ສະພາຜູ້ບໍລິຫານ"
    :breadcrumb="[
      'ໜ້າຫຼັກ',
      'ກ່ຽວກັບພວກເຮົາ',
      'ສະພາຜູ້ບໍລິຫານ',
      'ຄະນະກຳມະການຄຸ້ມຄອງຄວາມສ່ຽງ'
    ]"
    background-image="/aboutus/navigatormission-bg.png"
  />

  <div class="boardofdirector">
    <bod_navbar />
  </div>

  <!-- ✅ pan เฉพาะ org-layout -->
  <div ref="orgLayout" class="org-layout">
    <div class="org-shell">
      <header class="org-header">
        <h2>ຄະນະກຳມະການຄຸ້ມຄອງຄວາມສ່ຽງ</h2>

        <!-- ✅ ทำให้เหมือนหน้าอื่น -->
        <p class="org-subline">
          <img src="/logolapnet/fullcircle.png" alt="" class="lapnet-logo" />
          Lao National Payment Network CO., LTD
        </p>
      </header>

      <!-- VERTICAL SIDE STRUCTURE (2 NODES) -->
      <div class="org-vertical">
        <!-- Node 1 -->
        <div class="org-node">
          <div class="connector-dot"></div>

          <div class="person-card member-card">
            <div class="avatar-wrapper">
              <div class="avatar">
                <img src="/board-director-profile/bic.png" alt="" />
              </div>
            </div>

            <div class="person-info">
              <div class="bank-meta">
                <img
                  class="bank-logo"
                  src="/logoallmember/circle_scale/BIC.png"
                  alt="BIC"
                />
                <span class="bank-name">ທະນາຄານ ບີໄອຊີ ລາວ ຈຳກັດ</span>
              </div>

              <div class="name">ທ່ານ ຟີລິກ ດີຟຣານຊິສ</div>
              <div class="position">
                ຫົວໜ້າຄະນະກຳມະການຄຸ້ມຄອງຄວາມສ່ຽງ
              </div>
            </div>
          </div>
        </div>

        <!-- Node 2 -->
        <div class="org-node">
          <div class="connector-dot"></div>

          <div class="person-card member-card">
            <div class="avatar-wrapper">
              <div class="avatar">
                <img src="/board-director-profile/lvb.png" alt="" />
              </div>
            </div>

            <div class="person-info">
              <div class="bank-meta">
                <img
                  class="bank-logo"
                  src="/logoallmember/circle_scale/lvb.PNG"
                  alt="LVB"
                />
                <span class="bank-name">ທະນາຄານ ຮ່ວມທຸລະກິດລາວ-ຫວຽດ</span>
              </div>

              <div class="name">ທ່ານ ວຽງວິໄລ ແສງຄຳຢອງ</div>
              <div class="position">
                ຮອງຫົວໜ້າຄະນະກຳມະການຄຸ້ມຄອງຄວາມສ່ຽງ
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- /VERTICAL SIDE STRUCTURE -->
    </div>
  </div>

  <div class="boxmargin"></div>
  <secondfooter />
</template>

<script setup>
import { ref, onMounted, onUnmounted, nextTick } from 'vue'
import { onBeforeRouteLeave } from 'vue-router'
import { gsap } from 'gsap'

import bod_navbar from '../../../Views/Aboutus/companystructure/navbarcompany/bod_navbar.vue'
import main_navbar from '../../../components/miannavbar/main_navbar.vue'
import secondfooter from '../../../components/footer/mainfooter/secondfooter.vue'

const orgLayout = ref(null)
let ctx = null

const PAN_DISTANCE = 90

onMounted(async () => {
  await nextTick()

  ctx = gsap.context(() => {
    const cards = gsap.utils.toArray('.person-card')

    const tl = gsap.timeline({
      defaults: { ease: 'power3.out', duration: 0.8 },
    })

    // ✅ Pan-in เฉพาะ org-layout (เข้าจากขวา)
    tl.fromTo(
      orgLayout.value,
      { opacity: 0, x: PAN_DISTANCE },
      { opacity: 1, x: 0, duration: 0.55, ease: 'power2.out' }
    )
      .from('.org-shell', { opacity: 0, y: 40, scale: 0.97 }, '-=0.25')
      .from('.org-header', { opacity: 0, y: 20 }, '-=0.45')
      .from(
        cards,
        { opacity: 0, y: 24, filter: 'blur(6px)', stagger: 0.1 },
        '-=0.35'
      )
  }, orgLayout.value)
})

// ✅ Pan-out เฉพาะ org-layout ก่อนเปลี่ยนหน้า
onBeforeRouteLeave((to, from, next) => {
  if (!orgLayout.value) return next()

  gsap.killTweensOf(orgLayout.value)
  gsap.to(orgLayout.value, {
    x: -PAN_DISTANCE,
    opacity: 0,
    duration: 0.45,
    ease: 'power2.inOut',
    onComplete: next,
  })
})

onUnmounted(() => {
  if (ctx) ctx.revert()
})
</script>

<style scoped>
.boardofdirector {
  width: 100%;
  height: 15vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #f6f6f6;
}

.boxmargin {
  width: 100%;
  height: 15vh;
  background-color: #f6f6f6;
}

.org-layout {
  min-height: 70vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: clamp(2rem, 4vw, 3.5rem) clamp(1.25rem, 4vw, 2.5rem);
  background: #f6f6f6;
  box-sizing: border-box;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'SF Pro Text',
    sans-serif;

  /* ✅ improve pan performance */
  will-change: transform, opacity;

  --chip-bg: linear-gradient(
    180deg,
    rgba(255, 255, 255, 0.9),
    rgba(241, 245, 249, 0.9)
  );
  --chip-border: rgba(37, 99, 235, 0.18);
  --chip-glow: rgba(37, 99, 235, 0.18);
}

.org-shell {
  width: 100%;
  max-width: 900px;
  background: #ffffff;
  border-radius: 2.8rem;
  padding: 3rem 3.2rem 3rem;
  position: relative;
  overflow: hidden;
  margin-inline: auto;
  box-shadow: 0 32px 90px rgba(15, 23, 42, 0.22),
    0 0 0 1px rgba(148, 163, 184, 0.28);
}

/* background glow */
.org-shell::before {
  content: '';
  position: absolute;
  inset: -40% 20%;
  background: var(--blue-gradient);
  opacity: 0.22;
  pointer-events: none;
  z-index: -1;
}

/* header */
.org-header {
  margin-bottom: 2.4rem;
}

.org-header h2 {
  margin: 0;
  font-size: clamp(1.6rem, 2vw, 2rem);
  font-weight: 600;
  color: #0f172a;
  letter-spacing: 0.03em;
}

.org-subline {
  margin: 0.5rem 0 0;
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: clamp(0.95rem, 1.1vw, 1.05rem);
  color: #6b7280;
}

.lapnet-logo {
  width: 25px;
  height: 25px;
}

/* VERTICAL SIDE STRUCTURE --------------------------------------- */
.org-vertical {
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 2rem;
  padding-left: 2.8rem;
}

/* vertical line */
.org-vertical::before {
  content: '';
  position: absolute;
  left: 1.4rem;
  top: 0.6rem;
  bottom: 0.6rem;
  width: 2px;
  background: linear-gradient(
    180deg,
    rgba(0, 51, 171, 0.15),
    rgba(37, 99, 235, 0.8)
  );
}

/* one row/node in the vertical line */
.org-node {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
}

/* dot on the line */
.connector-dot {
  width: 16px;
  height: 16px;
  border-radius: 999px;
  background: #ffffff;
  border: 3px solid #1d4ed8;
  box-shadow: 0 0 0 4px rgba(191, 219, 254, 0.7),
    0 10px 20px rgba(15, 23, 42, 0.25);
  margin-top: 1.3rem;
  flex-shrink: 0;
}

/* PERSON CARD ---------------------------------------------------- */
.person-card {
  flex: 1;
  display: flex;
  align-items: center;
  gap: 1.4rem;
  padding: 1.4rem 1.8rem;
  background: #ffffff;
  border-radius: 1.9rem;
  border: 1px solid #e5e7eb;
  box-shadow: 0 24px 50px rgba(15, 23, 42, 0.14),
    0 0 0 1px rgba(148, 163, 184, 0.16);
  box-sizing: border-box;
  transition: transform 0.16s ease-out, box-shadow 0.16s ease-out,
    border-color 0.16s ease-out, background 0.16s ease-out;
}

.person-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 30px 60px rgba(15, 23, 42, 0.26),
    0 0 0 1px rgba(37, 99, 235, 0.6);
  border-color: rgba(37, 99, 235, 0.7);
}

/* avatar left */
.avatar-wrapper {
  flex: 0 0 82px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.avatar {
  width: 82px;
  height: 82px;
  border-radius: 999px;
  background: var(--blue-gradient);
  box-shadow: 0 22px 40px rgba(15, 23, 42, 0.45),
    0 0 0 4px rgba(255, 255, 255, 0.96);
  overflow: hidden;
}

.avatar img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  border-radius: 50%;
}

/* text */
.person-info {
  display: flex;
  flex-direction: column;
  min-width: 0;
}

/* BANK META */
.bank-meta {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  padding: 7px 12px;
  border-radius: 999px;
  background: var(--chip-bg);
  border: 1px solid var(--chip-border);
  box-shadow: 0 10px 22px rgba(15, 23, 42, 0.08),
    0 0 0 3px var(--chip-glow);
  width: fit-content;
  max-width: 100%;
  margin-bottom: 0.75rem;
  transition: transform 0.16s ease, box-shadow 0.16s ease,
    border-color 0.16s ease;
}

.person-card:hover .bank-meta {
  border-color: rgba(37, 99, 235, 0.35);
  box-shadow: 0 14px 28px rgba(15, 23, 42, 0.12),
    0 0 0 4px rgba(37, 99, 235, 0.18);
  transform: translateY(-1px);
}

.bank-logo {
  width: 28px;
  height: 28px;
  border-radius: 999px;
  background: #ffffff;
  padding: 4px;
  box-shadow: 0 10px 18px rgba(15, 23, 42, 0.14);
  object-fit: contain;
  flex-shrink: 0;
}

.bank-name {
  font-size: 0.95rem;
  font-weight: 650;
  color: #0f172a;
  letter-spacing: 0.02em;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

/* name + position */
.name {
  font-size: 1.3rem;
  font-weight: 600;
  color: #111827;
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
}

.position {
  margin-top: 0.28rem;
  font-size: 1.05rem;
  color: #6b7280;
}

/* members */
.member-card {
  background: #f3f4f6;
}

/* ---------- Responsive ---------- */
@media (max-width: 900px) {
  .boardofdirector,
  .boxmargin {
    height: 10vh;
  }

  .org-shell {
    padding: 2.2rem 1.8rem 2.4rem;
    border-radius: 2.4rem;
  }

  .org-vertical {
    padding-left: 2.1rem;
  }

  .org-vertical::before {
    left: 1.1rem;
  }

  .avatar-wrapper {
    flex-basis: 64px;
  }

  .avatar {
    width: 64px;
    height: 64px;
  }

  .name {
    font-size: 1.15rem;
  }

  .position {
    font-size: 0.95rem;
  }

  .bank-logo {
    width: 26px;
    height: 26px;
  }

  .bank-name {
    font-size: 0.92rem;
  }
}

@media (max-width: 600px) {
  .boardofdirector,
  .boxmargin {
    height: 8vh;
  }

  .org-layout {
    padding: 1.5rem 1rem;
  }

  .org-shell {
    padding-inline: 1.25rem;
    padding-block: 1.7rem 2.8rem;
    
    border-radius: 2rem;
    box-shadow: 0 20px 40px rgba(15, 23, 42, 0.16),
      0 0 0 1px rgba(148, 163, 184, 0.18);
  }

  .org-header {
    margin-bottom: 1.4rem;
  }

  .org-vertical {
    gap: 1.35rem;
    padding-left: 1.9rem;
  }

  .person-card {
    flex-direction: column;
    align-items: center;
    text-align: center;
    padding: 1.1rem 1.1rem;
    gap: 0.85rem;
    border-radius: 1.6rem;
  }

  .person-info {
    display: contents;
  }

  .bank-meta {
    order: 1;
    width: 100%;
    justify-content: center;
    padding: 7px 10px;
    margin-bottom: 0.65rem;
  }

  .avatar-wrapper {
    order: 2;
    flex: 0 0 auto;
    margin-top: 0.15rem;
  }

  .name {
    order: 3;
    width: 100%;
    text-align: center;
    white-space: normal;
    overflow: visible;
  }

  .position {
    order: 4;
    width: 100%;
    text-align: center;
  }

  .bank-name {
    white-space: normal;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .connector-dot {
    margin-top: 1.15rem;
  }
}

/* ---------- Extra small phones ---------- */
@media (max-width: 480px) {
  .org-layout {
    padding: 1.15rem 0.75rem;
  }

  .org-shell {
    border-radius: 1.55rem;
    padding-inline: 1rem;
    padding-block: 1.45rem 2.5rem;
  }

  .org-node {
    gap: 0.75rem;
  }

  .org-vertical {
    padding-left: 1.7rem;
  }

  .org-vertical::before {
    left: 1.02rem;
  }

  .person-card {
    padding: 0.95rem 0.95rem;
    border-radius: 1.35rem;
  }

  .avatar {
    width: 56px;
    height: 56px;
  }

  .bank-logo {
    width: 24px;
    height: 24px;
    padding: 3px;
  }

  .bank-name {
    font-size: 0.9rem;
    -webkit-line-clamp: 3;
  }

  .name {
    font-size: 1.05rem;
  }

  .position {
    font-size: 0.92rem;
  }
}
</style>
