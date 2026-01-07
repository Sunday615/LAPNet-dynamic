<script setup>
import navbar from '../../components/miannavbar/main_navbar.vue';

import tagproduct from '../../components/tagproduct/tagproduct.vue';
import product4_footerlogomember from '../../components/footer/logomemberfooter/product4_footerlogomember.vue';
import product4tech from '../techbenetfit/product4tech.vue';
import logofloating from '../../components/logofloating/logofloating.vue';
import tablefee from '../../components/tablefee/tablefee.vue';
import hero_sectionproduct4 from '../../Views/products/product_herosection/hero_sectionproduct4.vue'
import secondfooter from '../../components/footer/mainfooter/secondfooter.vue';
import iconfloating from './iconfloat/iconfloating.vue';
import { onMounted } from 'vue';
onMounted(() => {
  window.scrollTo({
    top: 0,
    left: 0,
    behavior: 'smooth'
  });
});
const heroVideo = '/videos/productvdo-background.mp4'

const memberLogos = [
   {
      src: "/logoallmember/circle_scale/BCEL.png",
      alt: "BCEL Bank logo",
   },
    {
      src: "/logoallmember/circle_scale/APBB.PNG",
      alt: "APB Bank logo",
   },
   {
      src: "/logoallmember/circle_scale/LDB.PNG",
      alt: "LDB Bank logo",
   },
  
     {
      src: "/logoallmember/circle_scale/lvb.PNG",
      alt: "LVB Bank logo",
   },
   {
      src: "/logoallmember/circle_scale/JDB.png",
      alt: "JDB Bank logo",
   },
   {
      src: "/logoallmember/circle_scale/STB.png",
      alt: "STB Bank logo",
   },
   {
      src: "/logoallmember/circle_scale/BIC.png",
      alt: "BIC Bank logo",
   },
  
 
   {
      src: "/logoallmember/circle_scale/ICBC.png",
      alt: "ICBC Bank logo",
   },
   {
      src: "/logoallmember/circle_scale/BOC.png",
      alt: "BOC Bank logo",
   },
   {
      src: "/logoallmember/circle_scale/VTB.png",
      alt: "VTB Bank logo",
   },
   {
      src: "/logoallmember/circle_scale/IB.png",
      alt: "IB Bank logo",
   },
   {
      src: "/logoallmember/circle_scale/ACLB.png",
      alt: "ACLB Bank logo",
   },
    {
      src: "/logoallmember/circle_scale/Maruhan.png",
      alt: "Maruhan Bank logo",
   },
   
   {
      src: "/logoallmember/circle_scale/SACOM.PNG",
      alt: "SACOM Bank logo",
   },
   
   {
      src: "/logoallmember/circle_scale/Kasikorn.png",
      alt: "KBANK Bank logo",
   },
   {
      src: "/logoallmember/circle_scale/BFL.png",
      alt: "BFL Bank logo",
   },
   {
      src: "/logoallmember/circle_scale/PSVB.png",
      alt: "PSVB Bank logo",
   },
   {
      src: "/logoallmember/circle_scale/mb.png",
      alt: "MB Bank logo",
   },
     {
      src: "/logoallmember/circle_scale/mmoney.png",
      alt: "M-Money logo",
   },
   {
      src: "/logoallmember/circle_scale/umoney.png",
      alt: "U-Money logo",
   },
   // ...continue until you have 16 logos, all with their own paths
];
</script>

<template>
   <navbar title="ຜະລິດຕະພັນ ແລະ ການບໍລິການ" :breadcrumb="[
      'ໜ້າຫຼັກ',
      'ຜະລິດຕະພັນ ແລະ ການບໍລິການ',
      'ໂອນເງິນຂ້າມທະນາຄານເທິງມືຖື'
   ]" background-image="/overlaynav/product/navigatorcontent-bg.png" />
   <div class="productdiscription">
      <hero_sectionproduct4/>
   </div>
   <div class="benetfix">
      <product4tech />
   </div>
       
   <div class="condition_for_use">
    <iconfloating/>
   </div>
      <div class="boxmargin" style="width: 100%; height:15vh"></div>
   <div class="tablefee">
      <tablefee />
   </div>
   <div class="boxmargin" style="width: 100%; height:15vh"></div>
   <div class="footermemberproduct1">
      <product4_footerlogomember :subtitle="`  ການໂອນເງິນຂ້າມທະນາຄານເທິງມືຖື (Fund Transfer via Mobile
            Application) ໂດຍນຳໃຊ້ລະບົບ LMPS ໃນການເຊື່ອມໂຍງຜ່ານ
            Applications ຂອງແຕ່ລະທະນາຄານທີ່ເປັນສະມາຊິກຂອງບໍລິສັດລາວ
            ເນເຊີນນໍເພເມັ້ນ ເນັດເວີກ ໃຫ້ສາມາດໃຊ້ຟັງຊັ່ນການດຳເນີນທຸລະກຳໂອນ
            ເງິນຂ້າມທະນາຄານໄດ້.`" :features="[
               ' ຂອບເຂດການໂອນເງິນຂ້າມທະນາຄານເທິງມືຖື ແມ່ນສາມາດໂອນເງິນແຕ່ 1.000 ກີບ ຫາ 200.000.000 ກີບ/ຄັ້ງ. ',

            ]" :logos="memberLogos" />
   </div>

   <secondfooter />
</template>


<style scoped>
.tablefee {
   width: 100%;
   height: auto;




}

@media (max-width: 720px) {
   

}

.tablefee {
   width: 100%;
   height: auto;
}

@media (max-width: 921px) {
   .logofloatings {
      display: none;
   }

   #titlecondition {
      width: 100%;
      height: 100%;
   }
}

@media (max-width: 1140px) {
   #redponsivetitlebox {
      font-size: 3.3rem;
      font-weight: bold;
   }
}

@media (max-width: 515) {
   #redponsivetitlebox {
      font-size: 2rem;
      font-weight: bold;
   }

}
.condition_for_use{
   width: 100%;
   

   height: auto;
}
.techbtn {
   width: 100%;
   height: 15%;

}

.warning p i {
   font-size: 10px;
   margin-right: 10px;
   color: rgb(0, 76, 255);

}

.warning p {
   font-weight: 500;
}

.warning {

   font-size: var(--fs-base);
   width: 100%;
   height: 15%;


}

.script {
   width: 100%;
   height: 15%;
   font-size: var(--fs-base);

}

.titlebox h1 {
   font-size: 5rem;
   font-weight: bold;

}

.titlebox {
   width: 100%;
   height: 20%;


}

.logofloatings {
   width: 45%;
   height: 90%;

}



.footermemberproduct1 {
   width: 100%;

   height: auto;


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