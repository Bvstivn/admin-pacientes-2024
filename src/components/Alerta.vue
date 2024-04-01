<template>
  <div
    :class="isError ? 'bg-red-500' : 'bg-green-500'"
    class="text-white text-center p-3 uppercase font-bold mb-3 rounded-md"
  >
    {{ alerta.mensaje }}
  </div>
</template>

<script setup>
//Vue
import { computed } from "vue";

//Computed
const isError = computed(() => props.alerta.tipo === 'error');

//Definiciones
const props = defineProps({
  alerta: {
    type: Object,
    required: true,
  },
});
</script>
