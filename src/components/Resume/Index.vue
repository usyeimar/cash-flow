<script setup lang="ts">
import {computed} from "vue";
import {currencyFormater} from "@/helpers";

const props = defineProps({
  label: {
    type: String
  },
  labelVisual: {
    type: String,
    default: null
  },
  amount: {
    type: Number,
    default: null
  },
  totalAmount: {
    type: Number,
  }
})

const amountVisual = computed(() => props.amount !== null ? props.amount : props.totalAmount)
const labelVisual = computed(() => props.labelVisual != null ? props.labelVisual : props.labelVisual)
const amountCurrency = computed(() => currencyFormater.format(amountVisual.value))


</script>

<template>
  <div>
    <main>
      <p>
        {{ label }}
      </p>

      <h1 class="title">
        {{ amountCurrency }}
      </h1>
      <div class="graphic">
        <slot name="graphic"/>
      </div>
      <div class="action">
        <slot name="action"/>
      </div>
    </main>
  </div>
</template>

<style scoped>
main {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
}

h1,
p {
  margin: 0;
  text-align: center;
}

h1 {
  margin-top: 14px;
  color: var(--brand-green);
}

.title {
  margin-top: 14px;
  color: var(--brand-green);
}

.graphic {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  padding: 48px 24px;
  box-sizing: border-box;
}
</style>