<script setup lang="ts">
import {toRefs} from "vue";
import Movement from "@/components/Movements/Movement.vue";
import type {MovementItem} from "@/types";

const props = defineProps({
  movements: {
    type: Array<MovementItem>,
    default: () => []
  }
})


const emit = defineEmits([
  'remove'
])


</script>

<template>
  <div class="movements">
    <h2 class="title">Historial</h2>
    <div class="content">
      <Movement
          v-for="{id,title,description,amount } in movements"
          :key="id"
          :id="id"
          :title="title"
          :description="description"
          :amount="amount"
          @remove="emit('remove',id)"
      >
      </Movement>
    </div>
  </div>
</template>

<style scoped>
.movements {
  max-height: 100%;
  padding: 0 8px;
  margin-bottom: 14px;
}

.title {
  margin: 8px 16px 24px 16px;
  color: var(--brand-blue);
}

.content {
  max-height: 68vh;
  display: flex;
  flex-direction: column;
  gap: 8px;
  overflow-y: scroll;
}
</style>