<template>
  <Teleport to="body">
    <div tabindex="-1" aria-hidden="true" class="text-white overflow-y-auto overflow-x-hidden fixed top-0 right-0 left-0 z-50 w-full md:inset-0 h-[calc(100%-1rem)] max-h-full flex items-center justify-center p-4">
      <div class="relative p-4 w-full max-w-2xl max-h-full">
        <!-- Modal content -->
        <div class="relative rounded-lg shadow-sm bg-gray-700">
          <!-- Modal header -->
          <div class="flex items-center justify-between p-4 md:p-5 border-b rounded-t border-gray-600">
            <h3 class="text-xl font-semibold text-white">
              Розрахунки
            </h3>
            <button @click="onClose" type="button" class="text-gray-400 bg-transparent rounded-lg text-sm w-8 h-8 ms-auto inline-flex justify-center items-center hover:bg-gray-600 hover:text-white" data-modal-hide="default-modal">
              <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 14">
                <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6"/>
              </svg>
              <span class="sr-only">Close modal</span>
            </button>
          </div>
          <div class="p-4 md:p-5 space-y-4">
            <p class="text-base leading-relaxed">
              Твої грощі жлоб: <b class="text-white text-2xl ml-2">{{splitSalary[0]}}</b>
            </p>
            <p class="text-base leading-relaxed">
              Залишок: <b class="text-white text-2xl ml-2">{{splitSalary[1]}}</b>
            </p>
          </div>
          <div class="flex items-center p-4 md:p-5 border-t rounded-b border-gray-600">
            <AppButton text="Закрити" @click="onClose"/>
          </div>
        </div>
      </div>
    </div>
    <div class="bg-gray-900/80 fixed inset-0 z-40"/>
  </Teleport>
</template>

<script setup lang="ts">
import AppButton from '@/components/AppButton.vue'
import { computed } from 'vue'

type Props = {
  value?: number
}

const props = defineProps<Props>();

const emit = defineEmits(['close'])

const onClose = () => {
  emit('close')
}

const splitSalary = computed(() => {
  const amount = props.value || 0;
  const round = (x: number) => Math.round(x * 100) / 100;

  if (amount <= 85) return [round(amount), 0];

  if (amount <= 185) {
    const diff = amount - 85;
    return [round(85 + diff * 0.7), round(diff * 0.3)];
  }

  const rest = amount - 185;
  return [
    round(85 + 100 * 0.7 + rest * 0.5),
    round(100 * 0.3 + rest * 0.5),
  ];
})
</script>
