<script setup lang="ts">
import navbar from "../../components/miannavbar/main_navbar.vue";
import product6benetfit from "../techbenetfit/product6benetfit.vue";
import product6swiper from "../../Views/products/swipper/product6swiper.vue";
import crossborderbtn from "../../components/button/crossborderbtn.vue";
import product6tech from "../techbenetfit/product6tech.vue";
import secondfooter from "../../components/footer/mainfooter/secondfooter.vue";
import vdoproductcrossborderKH_LA from "../../components/videoproductcrossborder/vdoproductcrossborderKH_LA.vue";
import vdoproductcrossborderLA_KH from "../../components/videoproductcrossborder/vdoproductcrossborderLA_KH.vue";
import vdoproductcrossborderTH_LA from "../../components/videoproductcrossborder/vdoproductcrossborderTH_LA.vue";
import vdoproductcrossborderLA_TH from "../../components/videoproductcrossborder/vdoproductcrossborderLA_TH.vue";
import vdoproductcrossborderVN_LA from "../../components/videoproductcrossborder/vdoproductcrossborderVN_LA.vue";
import vdoproductcrossborderCN_LA from "../../components/videoproductcrossborder/vdoproductcrossborderCN_LA.vue";
import hero_sectionproduct6 from "../../Views/products/product_herosection/hero_sectionproduct6.vue";
import product6_footerlogomember from "../../components/footer/logomemberfooter/product6_footerlogomember.vue";

import { computed, ref, watch, onMounted } from "vue";
import { useRoute } from "vue-router";
import { gsap } from "gsap";

/** =========================
 * ✅ Load member logos from API
 * API: http://localhost:3000/api/members
 * Condition: crossborderproduct = 1
 * ========================= */
type MemberLogo = {
  src: string;
  alt: string;
  memberId?: number;
};

const API_BASE_URL = import.meta.env.VITE_API_BASE_URL || "http://localhost:3000";
const MEMBERS_API_URL = `${API_BASE_URL}/api/members`;

const memberLogos = ref<MemberLogo[]>([]);

const normalizeUrl = (p: unknown) => {
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

const getMemberId = (m: any) => Number(m?.idmember ?? m?.id ?? m?.member_id ?? 0);

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
      .filter((m: any) => String(m?.crossborderproduct) === "1" || m?.crossborderproduct === true)
      .sort((a: any, b: any) => getMemberId(a) - getMemberId(b))
      .map((m: any) => {
        const rawSrc =
          m?.image ?? m?.logo ?? m?.img ?? m?.photo ?? m?.path ?? m?.src ?? "";

        const alt =
          m?.alt ?? m?.name ?? m?.bank_name ?? m?.title ?? "Member logo";

        return {
          src: normalizeUrl(rawSrc),
          alt,
          memberId: getMemberId(m),
        };
      })
      .filter((x: MemberLogo) => !!x.src);
  } catch (err) {
    console.error("Error loading member logos (crossborderproduct):", err);
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

/** =========================
 * ✅ Video switch by route param
 * ========================= */
const route = useRoute();
const videoBox = ref<HTMLElement | null>(null);

// use only the param, not the whole path
const pair = computed(() => (route.params.pair as string) || "kh-la");

const currentVideoComponent = computed(() => {
  switch (pair.value) {
    case "kh-la":
      return vdoproductcrossborderKH_LA;
    case "la-kh":
      return vdoproductcrossborderLA_KH;
    case "th-la":
      return vdoproductcrossborderTH_LA;
    case "la-th":
      return vdoproductcrossborderLA_TH;
    case "vn-la":
    case "la-vn":
      return vdoproductcrossborderVN_LA;
    case "cn-la":
    case "la-cn":
      return vdoproductcrossborderCN_LA;
    default:
      return vdoproductcrossborderCN_LA;
  }
});

// only animate the video box when pair changes
watch(
  pair,
  () => {
    if (!videoBox.value) return;

    gsap.fromTo(
      videoBox.value,
      { opacity: 0, y: 40 },
      {
        opacity: 1,
        y: 0,
        duration: 2.0,
        ease: "power3.out",
      }
    );
  },
  { immediate: true }
);
</script>

<template>
  <navbar
    title="ຜະລິດຕະພັນ ແລະ ການບໍລິການ"
    :breadcrumb="[
      'ໜ້າຫຼັກ',
      'ຜະລິດຕະພັນ ແລະ ການບໍລິການ',
      'ຊຳລະຂ້າມແດນໃນຮູບແບບ QR CODE ລະຫວ່າງປະເທດ'
    ]"
    background-image="/overlaynav/product/navigatorcontent-bg.png"
  />

  <div class="productdiscription">
    <hero_sectionproduct6 />
  </div>

  <div class="benetfix">
    <product6benetfit />
  </div>

  <div class="slidecontainer">
    <product6swiper />
  </div>

  <div class="crossborderinfo">
    <div class="boxmargin" style="width: 100%; height: 15vh"></div>

    <div class="titlecrossborder">
      <h1 id="memberjoin">ທະນາຄານສະມາຊິກທີ່ເຂົ້າຮ່ວມ</h1>
      <crossborderbtn />
    </div>

    <div class="videocontainer">
      <div class="videocorssbordershow" ref="videoBox">
        <component :is="currentVideoComponent" :key="pair" />
      </div>
    </div>
  </div>

  <div class="descriptionforcustomer">
    <product6tech />
  </div>

  <div class="footermember">
    <!-- ✅ ส่ง logos ที่ get จาก API (crossborderproduct=1) -->
    <product6_footerlogomember :subtitle="`  ຊຳລະຂ້າມແດນຜ່ານ QR CODE ບໍລິສັດ LAPNet ໄດ້ຕໍ່ຍອດການພັດທະນາລະບົບຊຳລະຂ້າມທະນາຄານເທິງມືຖື (LMPS) ເພື່ອຂະຫຍາຍຜະລິດຕະພັນ ຊຳລະຂ້າມທະນາຄານ ໃຫ້ສາມາດຮອງຮັບການ ຊຳລະ ຄ່າສິນຄ້າ - ບໍລິການ ລະຫວ່າງປະເທດ`"
      :features="[
        'ປັດຈຸບັນ LAPNet ໄດ້ຮ່ວມໂຄງການກັບ 4 ປະເທດຄື: ປະເທດກຳປູເຈຍ, ປະເທດໄທ, ປະເທດຈີນ ແລະ ປະເທດຫວຽດນາມ. ',
      ]"
      :logos="memberLogos" />
  </div>

  <secondfooter />
</template>

<style scoped>
.footermember {
  width: 100%;
  height: auto;
}

.descriptionforcustomer {
  width: 100%;
  height: auto;
}

.videocontainer {
  width: 100%;
  height: auto;
}

@media (max-width: 518px) {
  #memberjoin {
    font-size: 2.3rem;
  }
}

.titlecrossborder h1 {
  font-size: var(--fs-hero);
  font-weight: bold;
  color: #0025b8;
  background-image: linear-gradient(45deg, #312ba4, #0d1c9c 8%, #00a6ff 100%);
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  display: inline-block;
}

.titlecrossborder {
  width: 100%;
  height: 30%;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.crossborderinfo {
  width: 100%;
  height: auto;
}

@media (max-width: 768px) {
  .slidecontainer {
    display: none;
  }
}

.slidecontainer {
  width: 100%;
  height: 54vh;
}

.footermemberproduct1 {
  width: 100%;
  height: 100vh;
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
