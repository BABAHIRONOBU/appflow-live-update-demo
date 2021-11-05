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

  const config = await Deploy.getConfiguration();
  alert(JSON.stringify(config));

  const version = await Deploy.getCurrentVersion();
  alert(JSON.stringify(version));

  const versions = await Deploy.getAvailableVersions();
  alert(JSON.stringify(versions));
});
</script>
