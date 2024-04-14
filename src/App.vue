<script setup lang="ts">
import {defineAsyncComponent} from "vue";
import MySplashScreen from "@/components/SplashScreen.vue";
import TheHome from "@/views/Home.vue";

const MyMainApp = defineAsyncComponent<typeof TheHome>(() => new Promise((resolve) => {
  setTimeout(() => resolve(import("@/views/Home.vue")), 2000);
}));
</script>

<template>
  <Suspense>
    <template #default>
      <MyMainApp/>
    </template>
    <template #fallback>
      <MySplashScreen/>
    </template>
  </Suspense>
</template>

<style>
html,
body,
.app {
  min-height: 100vh;
  margin: 0;
  font-family: Arial, Helvetica, sans-serif;
}

* {
  --brand-green: #04b500;
  --brand-blue: #0689b0;
}
</style>