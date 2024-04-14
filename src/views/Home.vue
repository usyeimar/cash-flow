<script setup lang="ts">
import Layout from "@/components/Layout.vue";
import Header from "@/components/Header.vue";
import Resume from "@/components/Resume/Index.vue";
import Movements from "@/components/Movements/Index.vue";
import { computed, onMounted, onUpdated, ref } from "vue";
import type { MovementItem } from "@/types";
import Action from "@/components/Action.vue";
import Graphic from "@/components/Resume/Graphic.vue";

const movements = ref<MovementItem[]>([]);

const amounts = computed(() => {

  const lastDays = movements.value.filter((movement) =>{
    const today = new Date();
    const oldDate = today.setDate(today.getDate() - 30);
    return movement.time > oldDate;
  }).map((movement) => movement.amount);

  return lastDays.map((amount,i) => {
    const previousMovements = lastDays.slice(0,i);
    return  previousMovements.reduce((suma,movement) => suma + Number(movement),0);
  });

});

const addMovement = (movement: MovementItem) => {
  movements.value.push({
    ...movement,
    id: movements.value.length + 1,
    time: new Date()
  });


  save();

}

const deleteMovement = (id: number) => {
  const index = movements.value.findIndex((movement) => movement.id === id);
  movements.value.splice(index,1);
  save();
}

onMounted(() => {
  //get movements from local storage
  const movementsStorage = JSON.parse(localStorage.getItem('movements') || '[]');
  if(movementsStorage.length > 0){
    movements.value = movementsStorage;
  }
})

const save = () => {
  localStorage.setItem('movements',JSON.stringify(movements.value));
}
const totalAmoun = computed(() => {
  return movements.value.reduce((suma,movement) => suma + Number(movement.amount),0);
})

</script>

<template>
  <Layout>
    <!--Cabecera-->
    <template #header>
      <Header />
    </template>
    <!--Resumen-->
    <template #resume>
      <Resume 
      :label="'Ahorro Total'" 
      :total-label="'Ahorro total'" 
      :total-amount="totalAmoun">
        <template #graphic>
          <Graphic :amounts="amounts" @select-day="console.log" />
        </template>
        <template #action>
          <Action @add-movement="addMovement" />
        </template>
      </Resume>
    </template>

    <!--Movimientos -->
    <template #movements>
      <Movements :movements="movements"  @remove="deleteMovement" />
    </template>

  </Layout>
</template>

<style scoped></style>