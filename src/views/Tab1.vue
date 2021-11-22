<template>
  <ion-page>
    <ion-content :fullscreen="true">
    
      <HomePageSlider name="Tab 1 page" />
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonPage, IonContent } from '@ionic/vue';
import HomePageSlider from '@/components/HomePageSlider.vue';

export default  {
  name: 'Tab1',
  components: { HomePageSlider, IonContent, IonPage }
}
</script>