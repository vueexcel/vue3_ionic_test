<template>
    <ion-content>
      <swiper
        :modules="[Thumbs]"
        :controller="{ control: controlledSwiper }"
        :slides-per-view="1"
        :space-between="50"
        navigation
        :pagination="{ clickable: true }"
        style="height: 100%"
      >
        <swiper-slide>
          <div class="firstSlide">
            <img src="@/assets/Illustration.svg">
            <p>Connect easily with your family and friends over countries</p>
          </div>
        </swiper-slide>
        <swiper-slide>
          <div class="firstSlide">
            <img src="@/assets/privacy.jpg">
            <p>Enjoy while having full privacy with our end-to-end encryption</p>
          </div>
        </swiper-slide>
        <swiper-slide>
          <div class="firstSlide">
            <img src="@/assets/storage.jpg">
            <p>Store unlimited audio, video and text messages for free</p>
          </div>
        </swiper-slide>
      </swiper>
    </ion-content>
</template>
<script>
  import { defineComponent, ref } from 'vue';
  import { Controller, Thumbs  } from 'swiper';
  import { Swiper, SwiperSlide } from 'swiper/vue';
  import { IonContent } from '@ionic/vue';
  import 'swiper/swiper-bundle.min.css';
  import SwiperCore, { Navigation, Pagination } from 'swiper';

  SwiperCore.use([Navigation, Pagination]);

  export default defineComponent({
    components: { Swiper, SwiperSlide, IonContent },
    setup() {
      const controlledSwiper = ref(null)

      return {
        Controller,
        controlledSwiper,
        modules: [ Navigation, Pagination, Controller, Thumbs  ]
      }
    }
  });
</script>

<style scoped>
 .firstSlide{
   width:75%;
   text-align: center;
   margin:20% auto;
   font-family: Mulish;
 }
 .firstSlide p{
   font-weight : 700;
   font-style: normal;
   font-size: 24px;
   line-height: 30px;
 }
</style>