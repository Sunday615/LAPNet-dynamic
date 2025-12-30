<template>


  <!-- Desktop links -->
  <!-- <popupoverlay   
     v-model="show1"
    image-src="/blog/1.jpg"
    title="Popup #1 (Test)"
    description="Show popup 1"
    primary-text="Next"
    secondary-text="Skip"
    @closed="handleClosed1"
    @primary="handleClosed1"
    @secondary="handleClosed1"
    
    />
  <popupoverlay   
     v-model="show2"
    image-src="/footer/backgroundfooter.png"
    title="Popup #2 (Test)"
    description="Show popup 2"
    primary-text="Done"
    secondary-text="Close"
    @closed="handleClosed2"
    @primary="handleClosed2"
    @secondary="handleClosed2"
    
    /> -->


  
  <div class="herosectionhomepage">
    <mockupherosectionpage />
  </div>
  <div class="vision">
    <visionherosection />
  </div>
  <div class="whytus">
    <whychooseus title="Why choose LAPNET"
      description="ບໍລິສັດ ລາວເນ ເຊີນນໍ ເພເມັ້ນ ເນັດເວີກ ຈຳກັດ (LAPNet) ເຊິ່ງບໍລິສັດ LAPNet ເປັນຜູ້ໃຫ້ບໍລິການລະບົບ ການຊໍາລະທຸລະກໍາຍ່ອຍ ຕາມທີ່ໄດ້ກໍານົດໄວ້ໃນກົດໝາຍວ່າດ້ວຍລະບົບ ການຊໍາລະ. ຊຶ່ງເຮັດໜ້າທີ່ເປັນໂຕກາງໃນການ ເຊື່ອມໂຍງກັບທຸກຂະແໜງການຊຳລະທີ່ເປັນທະນາຄານ, ສະຖາບັນການເງິນ ແລະ ຜູ້ໃຫ້ບໍລິການ ການຊຳລະແຫ່ງດຽວໃນ ສປປ ລາວ;"
      :stat1Value="19" stat1Label="Commercial Bank"
      stat1Suffix="19 ທະນາຄານສະມາຊິກທີ່ເຂົ້າຮ່ວມກັບ Lao National Payment Network CO., LTD" :stat2Value="2"
      stat2Label="Fintech" stat2Suffix="" />
  </div>
  <div class="memberscroll">
    <allmemberscrolling />
  </div>
  <div class="heroproduct">
    <productherosectionhomepage />
  </div>
  <div class="boxpadding" style="width: 100%; height:15vh"></div>
  <div class="bloghomepage">
    <bloghomepage />
  </div>
  <div class="boxpadding" style="width: 100%; height:15vh"></div>
  <secondfooter />

</template>


<style scoped>
.herosectionhomepage {
  width: 100%;
  height: auto;

}

.globemockup {
  width: 100%;
  height: auto;
  background-color: #000;
}

.bloghomepage {
  width: 100%;
  height: auto;
  margin: 0 auto;

}

.heroproduct {
  max-width: 100%;
  height: auto;
  margin: 0 auto;
}

.memberscroll {
  max-width: 1380px;
  height: auto;
  margin: 0 auto;
}

.whyus {
  max-width: 1380px;
  height: auto;
  margin: 0 auto;
}

.vision {
  max-width: 100%;
  height: auto;
}
</style>
<script setup>
import mockupherosectionpage from './mockupherosectionpage.vue';
import atmmockup from '../../components/mockup/atmmockup.vue';
import bloghomepage from '../../components/blog/hompage/bloghomepage.vue';
import main_navbar from '../../components/miannavbar/main_navbar.vue';
import visionherosection from './visionherosection.vue';
import whychooseus from './whychooseus.vue';
import allmemberscrolling from '../../components/swiper/memberscrolling/allmemberscrolling.vue';
import productherosectionhomepage from './productherosectionhomepage.vue';
import secondfooter from '../../components/footer/mainfooter/secondfooter.vue';
import popupoverlay from '../../components/popup/popupoverlay.vue';
import { ref, onMounted } from "vue"


/** ✅ TEST MODE: 3 Hour per show  */
const INTERVAL_MS = 3 * 60 * 60 * 1000 // 3 hours

const KEY_LAST = "home_popups_last_shown_ts_test"
const KEY_STATE = "home_two_popups_state_test"

const show1 = ref(false)
const show2 = ref(false)

function getNumber(key) {
  try {
    const v = localStorage.getItem(key)
    if (!v) return null
    const n = Number(v)
    return Number.isFinite(n) ? n : null
  } catch {
    return null
  }
}

function setValue(key, val) {
  try {
    localStorage.setItem(key, String(val))
  } catch {}
}

function shouldShowNow() {
  const last = getNumber(KEY_LAST)
  if (!last) return true
  return Date.now() - last >= INTERVAL_MS
}

function markShownNow() {
  setValue(KEY_LAST, Date.now())
}

function setState(val) {
  setValue(KEY_STATE, val)
}

onMounted(() => {
  // ยังไม่ครบ 2 นาที → ไม่โชว์
  if (!shouldShowNow()) return

  // ครบแล้ว → บันทึกเวลาทันที (กันรีเฟรชเด้งซ้ำ)
  markShownNow()

  // เริ่มรอบใหม่จาก popup1 เสมอ
  setState("step1")

  // หน่วงตอนโหลดหน้า (ปรับได้)
  setTimeout(() => {
    show1.value = true
  }, 500)
})

function handleClosed1() {
  show1.value = false
  setState("step2")
  setTimeout(() => {
    show2.value = true
  }, 200)
}

function handleClosed2() {
  show2.value = false
  setState("done")
}
</script>