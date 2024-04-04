<template>
  <div class="md:w-1/2">
    <h2 class="font-black text-3xl text-center">Seguimiento Pacientes</h2>
    <p class="text-lg mt-5 text-center mb-10">
      Añade Pacientes y
      <span class="text-indigo-600 font-bold">Adminístralos</span>
    </p>
    <Alerta v-if="alerta.mensaje" :alerta="alerta" />
    <form
      @submit.prevent="validar"
      class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
    >
      <div class="mb-5">
        <label for="mascota" class="block text-gray-700 uppercase font-bold"
          >Nombre Mascota</label
        >
        <input
          for="mascota"
          type="text"
          placeholder="Nombre de la mascota"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="nombreMascota"
          @input="$emit('update:nombreMascota', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="propietario" class="block text-gray-700 uppercase font-bold"
          >Nombre Propietario</label
        >
        <input
          for="propietario"
          type="text"
          placeholder="Nombre del propietario"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="nombrePropietario"
          @input="$emit('update:nombrePropietario', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="email" class="block text-gray-700 uppercase font-bold"
          >Email</label
        >
        <input
          for="email"
          type="email"
          placeholder="Email"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="email"
          @input="$emit('update:email', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="alta" class="block text-gray-700 uppercase font-bold"
          >Alta</label
        >
        <input
          for="alta"
          type="date"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="alta"
          @input="$emit('update:alta', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="sintomas" class="block text-gray-700 uppercase font-bold"
          >Síntomas</label
        >
        <textarea
          for="sintomas"
          placeholder="Describe los síntomas"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40"
          :value="sintomas"
          @input="$emit('update:sintomas', $event.target.value)"
        />
      </div>

      <input
        type="submit"
        class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo-700 cursor-pointer transition-colors"
        :value="editando ? 'Guardar Cambios' : 'Registrar Paciente'"
      />
    </form>
  </div>
</template>

<script setup>
//Vue
import { reactive, computed } from "vue";
//Components
import Alerta from "@/components/Alerta.vue";

//Definiciones

const props = defineProps({
  nombrePropietario:{
    type: String,
    required: true
  },
  nombreMascota:{
    type: String,
    required: true
  },
  alta:{
    type: String,
    required: true
  },
  email:{
    type: String,
    required: true
  },
  sintomas:{
    type: String,
    required: true
  },
  id:{
    type: [String, null],
    required: true,
  }
});

const emit = defineEmits(['update:nombrePropietario', 'update:nombreMascota', 'update:alta', 'update:email', 'update:sintomas', 'guardar-paciente']);

//Data

const alerta = reactive({
  tipo: "",
  mensaje: "",
});

const editando = computed(() => props.id);

//Methods
const validar = () => {
  if (Object.values(props).includes("")) {
    alerta.mensaje = "Todos los campos son obligatorios";
    alerta.tipo = "error";
    return;
  }
  emit('guardar-paciente');
  alerta.mensaje = 'Paciente Almacenado Correctamente';
  alerta.tipo = 'exito';

  setTimeout(() => {
    alerta.tipo = '';
    alerta.mensaje = '';
  }, 3000);
};
</script>
