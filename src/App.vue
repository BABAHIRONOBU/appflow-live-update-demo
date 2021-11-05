<template>
  <ion-app>
    <ion-router-outlet />

    <ion-alert
      :is-open="isOpen"
      header="새로운 버전을 설치합니다."
      :message="`${downloadProgress}%`"
      :buttons="buttons"
      @didDismiss="setOpen(false)"
    />
  </ion-app>
</template>

<script lang="ts" setup>
import { ref, onMounted } from 'vue';
import {
  IonApp, IonRouterOutlet, IonAlert, AlertButton,
} from '@ionic/vue';
import { Capacitor } from '@capacitor/core';
import { Deploy } from 'cordova-plugin-ionic';

const isOpen = ref(false);
const setOpen = (v = true) => {
  isOpen.value = v;
};

const downloadProgress = ref(0);
const setDownloadProgress = (v: number) => {
  downloadProgress.value = v;
};

const buttons: AlertButton[] = [
  {
    text: '확인',
    handler: async () => {
      await Deploy.downloadUpdate((progress) => {
        if (!progress) {
          return;
        }
        setDownloadProgress(progress);
      });
      await Deploy.extractUpdate();
      await Deploy.reloadApp();
    },
  },
];

onMounted(async () => {
  if (!Capacitor.isNativePlatform) {
    return;
  }

  const update = await Deploy.checkForUpdate();

  setOpen(update.available);
});
</script>
