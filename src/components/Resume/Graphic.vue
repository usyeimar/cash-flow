<script setup lang="ts">
import { watch, computed, ref, toRefs } from "vue";


const props = defineProps({
  amounts: {
    type: Array,
    default: () => []
  }
});


const { amounts } = toRefs(props);

const amountToPixels = (amount: number) => {
  const max = Math.max(...amounts.value);
  const min = Math.min(...amounts.value);

  const amountAbs = amount + Math.abs(min);
  const minMax = Math.abs(max) + Math.abs(min);

  return 200 - ((amountAbs * 100) / minMax) * 2;
};

const zero = computed(() => {
  return amountToPixels(0)
});

const points = computed(() => {
  const total = amounts.value.length;
  return Array(total).fill(100).reduce((points, amount, index) => {
    const x = (300 / total) * (index + 1);
    const y = amountToPixels(amount);
    return `${points} ${x},${y}`;
  }, "0,100");

});

const showPointer = ref(false);
const pointer = ref(0);
const emit = defineEmits(['select-day']);

watch(pointer, (value) => {
  const index = Math.ceil((value / (300 / amounts.value.length)) | 0);

  if (index < 0 || index > amounts.value.length) {
    return;
  }
  emit('select-day', amounts.value[index -1 ]);

});


const tap = ({ target, touches }) => {
  showPointer.value = true;
  const elementWidth = target.getBoundingClientRect().width;
  const elementX = target.getBoundingClientRect().x;
  const touchX = touches[0].clientX;
  const x = (touchX - elementX) * 300 / elementWidth;
  pointer.value = x;

  emit('select-day', x);

}

const untap = () => {
  showPointer.value = false;
}


</script>

<template>
  <div>
    <svg @touchstart="tap" @touchmove="tap" @touchend="untap" viewBox="0 0 300 200">
      <line stroke="#c4c4c4" stroke-width="2" x1="0" :y1="zero" x2="200" :y2="zero" />
      <polyline fill="none" :points="points" stroke="#0689B0" stroke-width="2" />
      <line v-show="showPointer" fill="none" stroke="#04b500" stroke-width="2" :x1="pointer" y1="0" :x2="pointer"
        y2="200" />

    </svg>
    <p>
      Ultimos 30 dias
    </p>

  </div>
</template>

<style scoped>
svg {
  width: 100%;
}

p {
  text-align: center;
}
</style>