<template>
  <div class="scrollingbar">
    <div class="titlemember" style="padding-left: 20px; padding-right: 20px;">
      <span>ທະນາຄານສະມາຊິກຂອງເຮົາ</span>
    </div>

    <!-- LEFT → RIGHT -->
    <div class="wrapper">
      <div class="itemLeft item1"><img src="/logoallmember/circle_scale/BCEL.png" alt="" /></div>
      <div class="itemLeft item2"><img src="/logoallmember/circle_scale/LDB.PNG" alt="" /></div>
      <div class="itemLeft item3"><img src="/logoallmember/circle_scale/APBB.PNG" alt="" /></div>
      <div class="itemLeft item4"><img src="/logoallmember/retangle_scale/jdb.png" alt="" /></div>
      <div class="itemLeft item5"><img  src="/logoallmember/circle_scale/MJBL.png" alt="" /></div>
      <div class="itemLeft item6"><img src="/logoallmember/retangle_scale/LVB.png" alt="" /></div>
      <div class="itemLeft item7"><img src="/logoallmember/retangle_scale/ICBC.svg.png" alt="" /></div>
      <div class="itemLeft item8"><img src="/logoallmember/retangle_scale/bboc.png" alt="" /></div>
      <div class="itemLeft item9">
        <img
          src="/logoallmember/retangle_scale/y-nghia-sau-sac-dang-sau-thiet-ke-logo-vietinbank-0.png"
          alt=""
        />
      </div>
      <div class="itemLeft item10"><img src="/logoallmember/retangle_scale/IB.png" alt="" /></div>
    </div>

    <!-- RIGHT → LEFT -->
    <div class="wrapper-right">
      <div class="itemRight item1-right"><img src="/logoallmember/retangle_scale/mmoney.png" alt="" /></div>
      <div class="itemRight item2-right"><img src="/logoallmember/retangle_scale/umoney.png" alt="" /></div>
      <div class="itemRight item3-right"><img src="/logoallmember/retangle_scale/aceleda.png" alt="" /></div>
      <div class="itemRight item4-right"><img src="/logoallmember/retangle_scale/BIC.png" alt="" /></div>
      <div class="itemRight item5-right"><img src="/logoallmember/retangle_scale/sacombank.png" alt="" /></div>
      <div class="itemRight item6-right"><img src="/logoallmember/retangle_scale/Stbretangle.png" alt="" /></div>
      <div class="itemRight item7-right"><img src="/logoallmember/retangle_scale/kkbank.PNG" alt="" /></div>
      <div class="itemRight item8-right"><img src="/logoallmember/retangle_scale/publicbank.png" alt="" /></div>
      <div class="itemRight item9-right"><img src="/logoallmember/retangle_scale/BFL.png" alt="" /></div>
      <div class="itemRight item10-right"><img src="/logoallmember/retangle_scale/psvb.PNG" alt="" /></div>
      <div class="itemRight item11-right"><img src="/logoallmember/retangle_scale/mbbank.png" alt="" /></div>
    </div>
  </div>
</template>

<style>
@import "tailwindcss";

.scrollingbar {
  width: 100%;
  height: 500px;
  margin: auto;
}

.titlemember span {
  background: linear-gradient(120deg, #0033ab 0%, #0078ff 40%, #00c6ff 100%);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}

.titlemember {
  margin-top: 55px;
  padding-bottom: 50px;
  text-align: center;
  font-size: var(--fs-xxl);
  font-family: "Noto Sans Lao", sans-serif;
  font-weight: bold;
}

/* =========================
   WRAPPERS + MASK
   ========================= */
.wrapper,
.wrapper-right {
  margin-top: 2rem;
  width: 90%;
  max-width: 1536px;
  margin-inline: auto;
  height: 100px;
  position: relative;
  overflow: hidden;

  mask-image: linear-gradient(
    to right,
    rgba(0, 0, 0, 0),
    rgba(0, 0, 0, 1) 18%,
    rgba(0, 0, 0, 1) 82%,
    rgba(0, 0, 0, 0)
  );
  -webkit-mask-image: linear-gradient(
    to right,
    rgba(0, 0, 0, 0),
    rgba(0, 0, 0, 1) 18%,
    rgba(0, 0, 0, 1) 82%,
    rgba(0, 0, 0, 0)
  );

  /* ✅ ปรับด้วย variables (กันช้อนกัน) */
  --itemW: 200px;
  --itemH: 100px;
  --endPos: -200px;
  --laneLeft: 2000px;  /* 200px * 10 */
  --laneRight: 2200px; /* 200px * 11 */
  --durLeft: 30s;
  --durRight: 30s;
}

/* =========================
   KEYFRAMES (ใช้ var)
   ========================= */
@keyframes scrollLeft {
  to {
    left: var(--endPos);
  }
}
@keyframes scrollRight {
  to {
    right: var(--endPos);
  }
}

/* =========================
   ITEMS (สำคัญ: box-sizing + img fit)
   ========================= */
.itemLeft,
.itemRight {
  width: var(--itemW);
  height: var(--itemH);
  border-radius: 10px;

  display: flex;
  justify-content: center;
  align-items: center;
  

  position: absolute;
  box-sizing: border-box; /* ✅ กัน padding ทำให้กว้างเกิน */
  overflow: hidden;       /* ✅ กันรูปใหญ่ล้นไปทับกัน */

  animation-timing-function: linear;
  animation-iteration-count: infinite;

  /* นิดๆ ให้ดูพรีเมียม */
  background: rgba(255, 255, 255, 0.06);
  border: 1px solid rgba(255, 255, 255, 0.08);
  backdrop-filter: blur(8px);
}

/* ✅ ทำให้ “ทุกโลโก้” ไม่ล้นกรอบ (คือสาเหตุหลักที่เหมือนช้อนกัน) */
.itemLeft img,
.itemRight img {
  width: 100%;

  height: 100%;
  object-fit: contain;
  padding: 10px; /* เว้นขอบให้โลโก้หายใจ */
  display: block;

}

/* =========================
   LEFT LINE (10 items)
   ========================= */
.itemLeft {
  left: max(var(--laneLeft), 100%);
  animation-name: scrollLeft;
  animation-duration: var(--durLeft);
}

/* delays (left) — ใช้ var(--durLeft) เพื่อไม่ให้ “ช้อน” ตอนปรับ duration */
.item1  { animation-delay: calc(var(--durLeft) / 10 * (10 - 1) * -1); }
.item2  { animation-delay: calc(var(--durLeft) / 10 * (10 - 2) * -1); }
.item3  { animation-delay: calc(var(--durLeft) / 10 * (10 - 3) * -1); }
.item4  { animation-delay: calc(var(--durLeft) / 10 * (10 - 4) * -1); }
.item5  { animation-delay: calc(var(--durLeft) / 10 * (10 - 5) * -1); }
.item6  { animation-delay: calc(var(--durLeft) / 10 * (10 - 6) * -1); }
.item7  { animation-delay: calc(var(--durLeft) / 10 * (10 - 7) * -1); }
.item8  { animation-delay: calc(var(--durLeft) / 10 * (10 - 8) * -1); }
.item9  { animation-delay: calc(var(--durLeft) / 10 * (10 - 9) * -1); }
.item10 { animation-delay: calc(var(--durLeft) / 10 * (10 - 10) * -1); }

/* =========================
   RIGHT LINE (11 items)
   ========================= */
.itemRight {
  right: max(var(--laneRight), 100%);
  animation-name: scrollRight;
  animation-duration: var(--durRight);
}

/* delays (right) — ใช้ var(--durRight) */
.item1-right  { animation-delay: calc(var(--durRight) / 11 * (11 - 1) * -1); }
.item2-right  { animation-delay: calc(var(--durRight) / 11 * (11 - 2) * -1); }
.item3-right  { animation-delay: calc(var(--durRight) / 11 * (11 - 3) * -1); }
.item4-right  { animation-delay: calc(var(--durRight) / 11 * (11 - 4) * -1); }
.item5-right  { animation-delay: calc(var(--durRight) / 11 * (11 - 5) * -1); }
.item6-right  { animation-delay: calc(var(--durRight) / 11 * (11 - 6) * -1); }
.item7-right  { animation-delay: calc(var(--durRight) / 11 * (11 - 7) * -1); }
.item8-right  { animation-delay: calc(var(--durRight) / 11 * (11 - 8) * -1); }
.item9-right  { animation-delay: calc(var(--durRight) / 11 * (11 - 9) * -1); }
.item10-right { animation-delay: calc(var(--durRight) / 11 * (11 - 10) * -1); }
.item11-right { animation-delay: calc(var(--durRight) / 11 * (11 - 11) * -1); }

/* =========================
   MOBILE: max-width 453px
   ✅ ลดโลโก้ลง + เพิ่มจำนวนที่เห็น + ไม่ให้ช้อน
   ========================= */
@media (max-width: 453px) {
  .scrollingbar {
    height: 380px;
  }

  .titlemember {
    margin-top: 36px;
    padding-bottom: 24px;
    font-size: var(--fs-memberxl);
  }

  .wrapper,
  .wrapper-right {
    height: 70px;
    width: 94%;

    --itemW: 120px;
    --itemH: 70px;
    --endPos: -120px;
    --laneLeft: 1200px;  /* 120px * 10 */
    --laneRight: 1320px; /* 120px * 11 */
    --durLeft: 26s;
    --durRight: 26s;
  }

  .itemLeft img,
  .itemRight img {
    padding: 7px; /* ให้โลโก้ใหญ่ขึ้นในกรอบเล็ก */
  }
}
</style>

<script>
</script>
