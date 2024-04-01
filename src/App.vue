<script setup>
//Vue
import { ref, reactive } from "vue";
//Components
import Header from "./components/Header.vue";
import Formulario from "./components/Formulario.vue";

//Data
const pacientes = ref([]);

const paciente = reactive({
  nombreMascota: "",
  nombrePropietario: "",
  email: "",
  alta: "",
  sintomas: "",
});

//Methods
const guardarPaciente = () => {
  pacientes.value.push(paciente);
}

</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />
    <div class="mt-12 md:flex">
      <Formulario
        v-model:email="paciente.email"
        v-model:sintomas="paciente.sintomas"
        v-model:alta="paciente.alta"
        v-model:nombrePropietario="paciente.nombrePropietario"
        v-model:nombreMascota="paciente.nombreMascota"
        @guardar-paciente="guardarPaciente"
      />
      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">
          Administra tus pacientes
        </h3>

        <div v-if="pacientes.length > 0"></div>
        <p v-else class="mt-20 text-2xl text-center">No hay Pacientes</p>
      </div>
    </div>
  </div>
</template>
