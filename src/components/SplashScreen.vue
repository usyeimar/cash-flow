<script setup lang="ts">

</script>

<template>
  <div class="splashscreen">
    <img src="@/assets/cashflow-icon.svg"
         alt="Cashflow Icon"
         width="100"
         height="100">

    <h1>
      Cash<span>Flow</span>
    </h1>
  </div>
</template>

<style scoped>
.splashscreen {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  height: 100vh;
}

h1 {
  color: var(--brand-blue);
}

h1 span {
  color: var(--brand-green);
}
</style>