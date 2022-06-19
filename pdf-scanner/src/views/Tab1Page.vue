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
            <ion-img :src="photo.webviewPath" @click="showActionSheet(photo)"></ion-img>
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
import { usePhotoGallery, UserPhoto, } from "../composables/usePhotoGallery";
import { defineComponent } from "vue";
import { camera, trash, close, create } from "ionicons/icons";

import {
  actionSheetController,
  IonPage,
  IonHeader,
  IonFab,
  IonFabButton,
  IonIcon,
  IonToolbar,
  IonTitle,
  IonContent,
  IonGrid,
  alertController,
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
    const { takePhoto, deletePhoto, photos, loadWorker, recognizeImage, convertJPGToPDF } = usePhotoGallery();
    const showActionSheet = async (photo: UserPhoto) => {
    const actionSheet = await actionSheetController.create({
    header: 'Photo',
    buttons: [
      {
        text: 'Delete',
        role: 'destructive',
        icon: trash,
        handler: () => {
          deletePhoto(photo);
        },
      },
      {
        text: 'Cancel',
        icon: close,
        role: 'cancel',
        handler: () => {
          // Nothing to do, action sheet is automatically closed
        },
      },
      {
        text: 'Save as PDF',
        icon: create,
        handler: () => {
          presentAlertPrompt();
        }
      }
    ],
  });
  const presentAlertPrompt = async () => {
    const alert = await alertController
      .create({
        header: 'Enter file name',
        inputs: [
          {
            name: 'pdfFileName'
          },
        ],
        buttons: [
          {
            text: 'Cancel',
            role: 'cancel',
            handler: () => {
              // Cancels action
            },
          },
          {
            text: 'Ok',
            handler: (alertData) => {
              convertJPGToPDF(photo, alertData.pdfFileName);
            },
          },
        ],
      });
    return alert.present();
  };
  await actionSheet.present();
};
    return {
      takePhoto,
      camera,
      trash,
      close,
      photos,
      showActionSheet,
      loadWorker,
      recognizeImage,
      convertJPGToPDF
    };
  },
});
</script>
