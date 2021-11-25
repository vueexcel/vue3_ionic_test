<template>
  <ion-page>
    <ion-header class="ion-no-border">
      <ion-toolbar>
        <ion-title>My profile</ion-title>
          <ion-icon :icon="createOutline" slot="end"/>

      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <div class="profileBox">
        <img src="https://i.pravatar.cc/150?img=24" />
        <div class="lighten">id5</div>
        <div class="nameBox"><h1>Helen Jobs</h1></div>
        <div class="lighten">19 year</div>
    </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonIcon } from '@ionic/vue';
import { createOutline } from 'ionicons/icons';

export default  {
  name: 'Tab3',
  components: { IonHeader, IonToolbar, IonTitle, IonContent, IonPage, IonIcon },
  setup() {
    return {
      createOutline
    }
  }
}
</script>

<style scoped>
ion-icon{
  font-size: 25px;
  margin-right: 5%;
  opacity: 0.5;
}
.profileBox{
  padding: 10% 10%;
  width: 100%;
  text-align: center;
}
.profileBox img{
  border-radius: 5%;
  height: 100px;
  width: 100px;
}
.nameBox{
  width: 25%;
  margin:1px auto;
}
.nameBox h1{
  margin-top: 0;
  margin-bottom: 0;
}
.lighten{
  margin:5px auto;
  opacity: 0.5;
  font-size: 0.8rem;
}
</style>