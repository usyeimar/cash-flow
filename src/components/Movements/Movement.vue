<script setup lang="ts">
import {computed, toRefs} from "vue";
import {currencyFormater} from "@/helpers";

const props = defineProps({
  id: {
    type: Number
  },
  title: {
    type: String
  },
  description: {
    type: String
  },
  amount: {
    type: Number
  }

})

const emit = defineEmits([
  'remove'
])

const {amount, id} = toRefs(props)

const amountCurrency = computed(() => currencyFormater.format(amount.value))
const removeItem = () => {
  emit('remove', id?.value)
}

const isNegative = computed(() => amount?.value < 0)
</script>

<template>
  <div class="movement">
    <div class="content">
      <h4>{{ title }}</h4>
      <p>{{ description }}</p>
    </div>
    <div class="action">
      <img src="@/assets/trash-icon.svg" alt="borrar" @click="removeItem">
      <p
          :class="{'red': isNegative,'green':!isNegative}"
      >
        {{ amountCurrency }}
      </p>
    </div>
  </div>
</template>

<style scoped>
.movement {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 16px;
  background-color: #e6f9ff;
  border-radius: 8px;
  box-sizing: border-box;
}

.movement .content {
  width: 100%;
}

.movement .action {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  flex-direction: column;
}

h4,
p {
  margin: 0;
  padding: 0;
}

h4 {
  margin-bottom: 8px;
}

.movement .action img {
  margin-bottom: 16px;
}

.red {
  color: red;
}

.green {
  color: green;
}
</style>