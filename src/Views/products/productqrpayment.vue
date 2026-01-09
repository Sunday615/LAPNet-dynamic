<script setup>
import { ref, onMounted } from "vue";

import main_navbar from "../../components/miannavbar/main_navbar.vue";

import tagproduct from "../../components/tagproduct/tagproduct.vue";
import product5_footerlogomember from "../../components/footer/logomemberfooter/product5_footerlogomember.vue";
import product5tech from "../techbenetfit/product5tech.vue";
import hero_sectionproduct5 from "../../Views/products/product_herosection/hero_sectionproduct5.vue";
import secondfooter from "../../components/footer/mainfooter/secondfooter.vue";

onMounted(() => {
  window.scrollTo({
    top: 0,
    left: 0,
    behavior: "smooth",
  });

  fetchMemberLogos();
});

const heroVideo = "/videos/productvdo-background.mp4";

/** =========================
 * ✅ Load member logos from API
 * API: http://localhost:3000/api/members
 * Condition: qrpayment = 1
 * ========================= */
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

const getMemberId = (m) => Number(m?.idmember ?? m?.id ?? m?.member_id ?? 0);

const fetchMemberLogos = async () => {
  try {
    const res = await fetch(MEMBERS_API_URL, {
      headers: { Accept: "application/json" },
    });
    if (!res.ok) throw new Error(`Fetch failed: ${res.status} ${res.statusText}`);

    const json = await res.json();
    const list = Array.isArray(json) ? json : json?.data || json?.members || [];
    const arr = Array.isArray(list) ? list : [];

    memberLogos.value = arr
      // ✅ where qrpayment = 1
      .filter((m) => String(m?.qrpayment) === "1" || m?.qrpayment === true)
      // ✅ order by idmember asc (ถ้าต้องการให้ idmember=1 ขึ้นบนสุดด้วย ก็จะขึ้นเองอยู่แล้ว)
      .sort((a, b) => getMemberId(a) - getMemberId(b))
      // ✅ map to {src, alt}
      .map((m) => {
        const rawSrc =
          m?.image ?? m?.logo ?? m?.img ?? m?.photo ?? m?.path ?? m?.src ?? "";

        const alt =
          m?.alt ?? m?.name ?? m?.bank_name ?? m?.title ?? "Member logo";

        return {
          src: normalizeUrl(rawSrc),
          alt,
        };
      })
      .filter((x) => !!x.src);
  } catch (err) {
    console.error("Error loading member logos:", err);
    memberLogos.value = [];
  }
};
</script>

<template>
  <main_navbar
    title="ຜະລິດຕະພັນ ແລະ ການບໍລິການ"
    :breadcrumb="[
      'ໜ້າຫຼັກ',
      'ຜະລິດຕະພັນ ແລະ ການບໍລິການ',
      'ການຊໍາລະເງິນຂ້າມທະນາຄານຜ່ານ QR'
    ]"
    background-image="/overlaynav/product/navigatorcontent-bg.png"
  />

  <div class="productdiscription">
    <hero_sectionproduct5 />
  </div>

  <div class="benetfix">
    <product5tech />
  </div>

  <div class="footermemberproduct1">
    <product5_footerlogomember
      :subtitle="`   ການຊໍາລະຂ້າມທະນາຄານຜ່ານ LAPNet LAOQR ໂດຍນຳໃຊ້ລະບົບ LMPS ໃນການເຊື່ອມໂຍງຜ່ານ Applications ຂອງແຕ່ລະທະນາຄານທີ່ເປັນສະມາຊິກຂອງບໍລິສັດ LAPNet ໃຫ້ສາມາດນຳໃຊ້ຟັງຊັ່ນການດຳເນີນທຸລະກຳຊໍາລະຜ່ານ QR ຂອງທະນາຄານອື່ນໄດ້.`"
      :features="[
        '  ການຊໍາລະຂ້າມທະນາຄານຜ່ານ LAPNet LAOQR ໂດຍນຳໃຊ້ລະບົບ LMPS',
      ]"
      :logos="memberLogos"
    />
  </div>

  <secondfooter />
</template>

<style scoped>
.footermemberproduct1 {
  width: 100%;
  height: auto;
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

.productdiscription {
  width: 100%;
  height: auto;
}
</style>
