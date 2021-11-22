<template>
  <ion-page>
    <ion-content>
      <swiper
        :modules="modules"
        :keyboard="true"
        :pagination="true"
      >
        <swiper-slide>Slide 1</swiper-slide>
        <swiper-slide>Slide 3</swiper-slide>
        <swiper-slide>Slide 3</swiper-slide>
      </swiper>
    </ion-content>
  </ion-page>
</template>
<script>
  import { defineComponent } from 'vue';
  import { Keyboard, Pagination } from 'swiper';
  import { Swiper, SwiperSlide } from 'swiper/vue';
  import { IonContent, IonPage } from '@ionic/vue';

  import 'swiper/css';
  import 'swiper/css/autoplay';
  import 'swiper/css/keyboard';
  import 'swiper/css/pagination';
  import 'swiper/css/scrollbar';
  import 'swiper/css/zoom';
  import '@ionic/vue/css/ionic-swiper.css';

  export default defineComponent({
    components: { Swiper, SwiperSlide, IonContent, IonPage },
    setup() {
      return {
        modules: [ Keyboard, Pagination ]
      }
    }
  });
</script>