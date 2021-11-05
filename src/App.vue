<template>
  <ion-app>
    <ion-router-outlet />

    <ion-alert
      :is-open="isOpen"
      :backdropDismiss="false"
      header="새로운 버전을 설치합니다."
      message="설치중입니다. 잠시만 기다려주세요."
      @didDismiss="setOpen(false)"
    />
  </ion-app>
</template>

<script lang="ts" setup>
import { ref, onMounted } from 'vue';
import { IonApp, IonRouterOutlet, IonAlert } from '@ionic/vue';
import { Capacitor } from '@capacitor/core';
import { Deploy } from 'cordova-plugin-ionic';

const isOpen = ref(false);
const setOpen = (v = true) => {
  isOpen.value = v;
};

onMounted(async () => {
  if (!Capacitor.isNativePlatform) {
    return;
  }

  const update = await Deploy.checkForUpdate();

  if (update.available) {
    setOpen();

    await Deploy.downloadUpdate((progress) => {
      console.log(`download ${progress}% completed`);
    });
    await Deploy.extractUpdate();
    await Deploy.reloadApp();
  }
});
</script>
