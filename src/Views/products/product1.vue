<script setup>
import { ref, onMounted } from "vue";

import navbar from "../../components/miannavbar/main_navbar.vue";
import product1_footerlogomember from "../../components/footer/logomemberfooter/product1_footerlogomember.vue";
import hero_sectionproduct1 from "./product_herosection/hero_sectionproduct1.vue";
import secondfooter from "../../components/footer/mainfooter/secondfooter.vue";
import product1tech from "../techbenetfit/product1tech.vue";

const API_BASE_URL = import.meta.env.VITE_API_BASE_URL || "http://localhost:3000";
const MEMBERS_API_URL = `${API_BASE_URL}/api/members`;

const memberLogos = ref([]);

const normalizeUrl = (p) => {
  if (!p || typeof p !== "string") return "";
  if (
    p.startsWith("http://") ||
    p.startsWith("https://") ||
    p.startsWith("data:") ||
    p.startsWith("blob:")
  ) {
    return p;
  }
  if (p.startsWith("/")) return `${API_BASE_URL}${p}`;
  return `${API_BASE_URL}/${p}`;
};

const fetchMemberLogos = async () => {
  try {
    const res = await fetch(MEMBERS_API_URL, {
      headers: { Accept: "application/json" },
    });
    if (!res.ok) throw new Error(`Fetch failed: ${res.status} ${res.statusText}`);

    const json = await res.json();
    const list = Array.isArray(json) ? json : (json?.data || json?.members || []);
    const arr = Array.isArray(list) ? list : [];

    memberLogos.value = arr
      // where atminquery = 1
      .filter((m) => String(m?.atminquery) === "1" || m?.atminquery === true)
      // ทำให้ idmember = 1 มาก่อน (ที่เหลือเรียงตาม idmember ปกติ)
      .sort((a, b) => {
        const ida = Number(a?.idmember ?? a?.id ?? a?.member_id ?? 0);
        const idb = Number(b?.idmember ?? b?.id ?? b?.member_id ?? 0);

        if (ida === 1 && idb !== 1) return -1;
        if (idb === 1 && ida !== 1) return 1;

        // fallback: เรียงตัวเลขน้อยไปมาก
        return ida - idb;
      })
      // map เป็น logos ที่ component ใช้
      .map((m) => {
        const rawSrc =
          m?.image ?? m?.logo ?? m?.img ?? m?.photo ?? m?.path ?? m?.src ?? "";

        return {
          src: normalizeUrl(rawSrc),
          alt: m?.alt ?? m?.name ?? m?.bank_name ?? m?.title ?? "Member logo",
        };
      })
      // เอาเฉพาะที่มีรูปจริง
      .filter((x) => !!x.src);
  } catch (err) {
    console.error("Error loading member logos:", err);
    memberLogos.value = [];
  }
};

onMounted(() => {
  window.scrollTo({
    top: 0,
    left: 0,
    behavior: "smooth",
  });

  fetchMemberLogos();
});
</script>

<template>
  <navbar
    title="ຜະລິດຕະພັນ ແລະ ການບໍລິການ"
    :breadcrumb="[
      'ໜ້າຫຼັກ',
      'ຜະລິດຕະພັນ ແລະ ການບໍລິການ',
      'ກວດຍອດເງິນຂ້າມທະນາຄານຜ່ານຕູ້ ATM'
    ]"
    background-image="/overlaynav/product/navigatorcontent-bg.png"
  />

  <div class="productdiscription">
    <hero_sectionproduct1 />
  </div>

  <div class="benetfix">
    <product1tech />
  </div>

  <div class="footermemberproduct1">
    <product1_footerlogomember :logos="memberLogos" />
  </div>

  <secondfooter />
</template>

<style scoped>
.footermemberproduct1 {
  width: 100%;
  height: 100vh;
}

.condition p {
  width: 100%;
  padding-top: 40px;

  width: 80%;
  font-size: var(--fs-base);
  text-align: center;
}
.condition h1 {
  text-align: center;

  width: 100%;
  font-size: var(--fs-xxl);
  font-weight: bold;
}
.condition {
  width: 100%;
  height: 40%;

  display: flex;
  flex-direction: column;
  align-items: center;
  color: #fff;
}
.benetfitcontent p {
  width: 100%;
  padding-top: 40px;

  width: 80%;

  font-size: var(--fs-base);
  text-align: center;
}
.benetfitcontent h1 {
  text-align: center;

  width: 100%;
  font-size: var(--fs-xxl);
  font-weight: bold;
}
.benetfitcontent {
  width: 100%;
  color: #fff;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 40%;
}
.benetfitcontainer {
  width: 80%;
  display: flex;
  justify-content: space-around;
  flex-direction: column;
  height: 100%;
}
.benetfix {
  width: 100%;
  height: auto;
}
.atmmockup img {
  width: 80%;
  position: relative;
  z-index: 1;
  left: -100px;
  bottom: -50px;
}
.description_right_container {
  width: 50%;
  height: 100%;
}
.topimg img {
  width: 30%;
  right: 40px;
  height: auto;
  object-fit: cover;
  border-radius: 10px;
  position: absolute;
  z-index: 0;
}
.description_left_container p {
  font-size: var(--fs-base);
}
.description_left_container h1 {
  font-size: var(--fs-xxxl);
  font-weight: bold;
  line-height: 1.2;
}
.description_left_container h3 {
  color: var(--color-second);
  font-weight: bold;
  font-size: var(--fs-md);
}
.description_right_container {
  width: 50%;
  height: 100%;
}
.description_left_container {
  width: 50%;
  display: flex;
  align-items: start;
  justify-content: space-evenly;
  flex-direction: column;

  height: 100%;
}
.productdiscription {
  width: 100%;
  height: auto;
}
</style>
