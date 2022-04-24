<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Scanner</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-grid>
        <ion-row>
          <ion-col size="6" :key="photo" v-for="photo in photos">
            <ion-img :src="photo.webviewPath"></ion-img>
          </ion-col>
        </ion-row>
      </ion-grid>
      <ion-fab vertical="bottom" horizontal="center" slot="fixed">
        <ion-fab-button @click="takePhoto()">
          <ion-icon :icon="camera"></ion-icon>
        </ion-fab-button>
      </ion-fab>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { usePhotoGallery, UserPhoto } from "@/composables/usePhotoGallery";
const { photos, takePhoto } = usePhotoGallery();
import { defineComponent } from "vue";
import { camera, trash, close } from "ionicons/icons";

import {
  IonPage,
  IonHeader,
  IonFab,
  IonFabButton,
  IonIcon,
  IonToolbar,
  IonTitle,
  IonContent,
  IonGrid,
} from "@ionic/vue";

export default defineComponent({
  name: "Tab1Page",
  components: {
    IonPage,
    IonHeader,
    IonFab,
    IonFabButton,
    IonIcon,
    IonToolbar,
    IonTitle,
    IonContent,
    IonGrid,
  },
  setup() {
    const { takePhoto } = usePhotoGallery();
    return {
      takePhoto,
      camera,
      trash,
      close,
      photos,
    };
  },
});
</script>
