<template>
  <button :type="type" class="bg-rose-700 text-white px-4 py-2 rounded w-[200px] mx-auto hover:bg-rose-900 focus:ring-rose-800 focus:outline-none focus:ring-4 default-transition">{{text}}</button>
</template>

<script setup lang="ts">
type Props = {
  text: string;
  type?: 'submit' | 'button';
}

withDefaults(defineProps<Props>(), {
  type: 'button'
})
</script>
