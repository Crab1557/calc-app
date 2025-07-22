<template>
  <div class="flex flex-col flex-1 items-center justify-center text-white py-8 px-4">
    <header class="max-w-[300px] text-center flex flex-col items-center gap-4 mb-6">
      <img alt="App logo" class="logo" src="./assets/images/logo.svg" width="75" height="75" />

      <h1 class="text-3xl text-center">
        Віктор лох міг би і сам порахувати
      </h1>
    </header>

    <main class="w-[300px]">
      <form @submit.prevent="onSubmit" class="flex flex-col">
        <div class="relative pb-6 mb-2">
          <label for="currency" class="block mb-2 text-base font-medium default-transition"
                 :class="{
              'text-red-500': error
            }"
          >
            Введи число для розрахунку
          </label>
          <input v-model.number="value"
                 type="number"
                 id="currency"
                 @input="onInput"
                 class="border text-sm rounded-lg focus:ring-red-500 bg-gray-700 block w-full p-2.5"
                 :class="{'focus:border-red-500  border-red-500 text-red-500 placeholder-red-500': error}"
                 placeholder="0">
          <Transition mode="out-in">
            <p v-if="error" class="text-sm text-red-600 dark:text-red-500 absolute bottom-0 left-0">Довбень введи нормальне число довбень</p>
          </Transition>
        </div>
        <AppButton text="Порахувати" type="submit"/>
      </form>
    </main>
    <Transition>
      <CalcModal v-if="isShowModal" @close="onCloseModal" :value="value"/>
    </Transition>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import AppButton from '@/components/AppButton.vue'
import CalcModal from '@/components/CalcModal.vue'

const error = ref<boolean>(false);
const isShowModal = ref(false)
const value = ref<number>();

const onValidate = (val: number) => {
  const isInvalid =  (!val || val < 0)
  error.value = isInvalid;

  return isInvalid;
}

const onInput = (e: Event) => {
  onValidate(Number((<HTMLInputElement>e.target).value))
}

const onSubmit = () => {
  if (!onValidate(value.value || 0)) {
    isShowModal.value = true;

    window?.confetti({
      ticks: 50,
      colors: ['f9f404', '0429f9', 'dbd715', '0452f9'],
      particleCount: 300,
      spread: 120,
      scalar: 3,
      origin: { y: 0.5, x: 0.5 },
      shapes: ['heart']
    });
  }
}

const onCloseModal = () => {
  isShowModal.value = false;
  value.value = undefined;
  error.value = false;
}
</script>
