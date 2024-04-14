<script setup lang="ts">

import {ref} from "vue";
import Modal from "@/components/Modal.vue";

const emit = defineEmits([
  'add-movement'
])
const showModal = ref(false)
const title = ref<string>('')
const description = ref<string>('')
const amount = ref<number>(0)

const movementType = ref('Ingreso')

const submit = () => {
  showModal.value =! showModal.value
  emit('add-movement',{
    title: title.value,
    description: description.value,
    amount: amount.value,
    movementType: movementType.value
  })

  title.value = ''
  description.value = ''
  amount.value = 0
  movementType.value = 'Ingreso'

  
}
</script>

<template>
  <button @click="showModal = true">
    Agregar movimiento
  </button>
  <teleport to="#app">
    <Modal v-show="showModal" @close="showModal = false">
      <form @submit.prevent="submit">
        <div class="field">
          <label>
            Titulo
          </label>
          <input type="text" v-model="title">
        </div>
        <div class="field">
          <label>
            Monto
          </label>
          <input type="number" v-model="amount">
        </div>

        <div class="field">
          <label>
            Descripcion
          </label>
          <textarea rows="3" type="text" v-model="description"/>
        </div>
        <div class="field">
          <label class="radio-label">
            <input type="radio" v-model="movementType" value="Ingreso">
            <span>
              Ingreso
            </span>
          </label>

          <label class="radio-label">
            <input type="radio" v-model="movementType" value="Gasto">
            <span>
              Gasto
            </span>
          </label>

          <div class="action">
            <button>
              Agregar movimiento
            </button>
          </div>

        </div>
      </form>
    </Modal>
  </teleport>
</template>

<style scoped>
button {
  color: white;
  font-size: 1.25rem;
  background-color: var(--brand-blue);
  border: none;
  width: 100%;
  padding: 24px 60px;
  border-radius: 60px;
  box-sizing: border-box;
}

form {
  font-size: 1.24rem;
  width: 100%;
}

form .action {
  padding: 0 24px;
}

.field {
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  padding: 16px 24px;
}

label {
  margin-bottom: 8px;
}

input,
textarea {
  font-size: 1.24rem;
  border: 2px solid var(--brand-blue);
  border-radius: 8px;
  padding: 8px;
}

input[type="number"] {
  text-align: right;
}

.radio-label {
  display: flex;
  align-items: center;
  margin-top: 8px;
}

.radio-label span {
  margin-top: 4px;
  margin-left: 8px;
}

input[type="radio"] {
  appearance: none;
  width: 1.24rem;
  height: 1.24rem;
  color: var(--brand-blue);
  border: 2px solid var(--brand-blue);
  border-radius: 50%;
}

input[type="radio"]:checked {
  background-color: var(--brand-blue);
}
</style>