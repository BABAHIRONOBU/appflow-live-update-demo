<template>
  <ion-app>
    <ion-router-outlet />
  </ion-app>
</template>

<script lang="ts" setup>
import { onMounted } from 'vue';
import { IonApp, IonRouterOutlet } from '@ionic/vue';
import { Capacitor } from '@capacitor/core';
import { Deploy } from 'cordova-plugin-ionic';

onMounted(async () => {
  if (!Capacitor.isNativePlatform) {
    return;
  }

  const update = await Deploy.checkForUpdate();

  if (update.available) {
    await Deploy.downloadUpdate((progress) => {
      console.log(progress);
    });
    await Deploy.extractUpdate((progress) => {
      console.log(progress);
    });
    await Deploy.reloadApp();
  }
});
</script>
