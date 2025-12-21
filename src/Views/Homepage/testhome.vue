<template>


    <!-- Desktop links -->
      
      <div class="herosectionhomepage">
          <mockupherosectionpage/>
      </div>
        <div class="vision">
          <visionherosection/>
        </div>
        <div class="whytus">
          <whychooseus
              title="Why choose LAPNET"
        description="ບໍລິສັດ ລາວເນ ເຊີນນໍ ເພເມັ້ນ ເນັດເວີກ ຈຳກັດ (LAPNet) ເຊິ່ງບໍລິສັດ LAPNet ເປັນຜູ້ໃຫ້ບໍລິການລະບົບ ການຊໍາລະທຸລະກໍາຍ່ອຍ ຕາມທີ່ໄດ້ກໍານົດໄວ້ໃນກົດໝາຍວ່າດ້ວຍລະບົບ ການຊໍາລະ. ຊຶ່ງເຮັດໜ້າທີ່ເປັນໂຕກາງໃນການ ເຊື່ອມໂຍງກັບທຸກຂະແໜງການຊຳລະທີ່ເປັນທະນາຄານ, ສະຖາບັນການເງິນ ແລະ ຜູ້ໃຫ້ບໍລິການ ການຊຳລະແຫ່ງດຽວໃນ ສປປ ລາວ;"
        :stat1Value="19"
        stat1Label="Commercial Bank"
        stat1Suffix="19 ທະນາຄານສະມາຊິກທີ່ເຂົ້າຮ່ວມກັບ Lao National Payment Network CO., LTD"
        :stat2Value="2"
        stat2Label="Fintech"
        stat2Suffix=""
          />
        </div>
        <div class="memberscroll">
          <allmemberscrolling/>
        </div>
        <div class="heroproduct">
          <productherosectionhomepage/>
        </div>
        <div class="boxpadding" style="width: 100%; height:15vh"></div>
        <div class="bloghomepage">
          <bloghomepage/>
        </div>
         <div class="boxpadding" style="width: 100%; height:15vh"></div>
        <secondfooter/>

</template>


<style scoped>
  .herosectionhomepage{
    width: 100%;
    height: auto;
    
  }
  .globemockup{
    width: 100%;
    height: auto;
    background-color: #000;
  }
  .bloghomepage{
    width: 100%;
    height: auto;
    margin: 0 auto;

  }
  .heroproduct{
    max-width: 100%;
    height: auto;
    margin: 0 auto;
  }
  .memberscroll{
    max-width: 1380px;
    height: auto;
    margin: 0 auto;
  }
  .whyus{
    max-width: 1380px;
    height: auto;
    margin: 0 auto;
  }
  .vision{
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
</script>