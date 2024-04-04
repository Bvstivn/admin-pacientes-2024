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
        :id="paciente.id"
        @guardar-paciente="guardarPaciente"
      />
      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">
          Administra tus pacientes
        </h3>

        <div v-if="pacientes.length > 0">
          <p class="text-lg mt-5 text-center mb-10">
            Información
            <span class="text-indigo-600 font-bold">Pacientes</span>
          </p>
          <Paciente
            v-for="paciente in pacientes"
            :paciente="paciente"
            @actualizar-paciente="actualizarPaciente"
            @eliminar-paciente="eliminarPaciente"
          />
        </div>
        <p v-else class="mt-20 text-2xl text-center">No hay Pacientes</p>
      </div>
    </div>
  </div>
</template>

<script setup>
//Vue
import { ref, reactive, watch, onMounted } from "vue";
//UID
import { uid } from "uid";
//Components
import Header from "./components/Header.vue";
import Formulario from "./components/Formulario.vue";
import Paciente from "./components/Paciente.vue";

//Data
const pacientes = ref([]);

const paciente = reactive({
  id: null,
  nombreMascota: "",
  nombrePropietario: "",
  email: "",
  alta: "",
  sintomas: "",
});

//Watchs

watch(pacientes, () => {
  guardarLocalStorage();
}, 
{
  deep: true,
});

//onMounted

onMounted(() => {
  const pacientesStorage = localStorage.getItem('pacientes');
  if(pacientesStorage){
    pacientes.value = JSON.parse(pacientesStorage);
  }
});

//Methods

const guardarLocalStorage = () => {
  localStorage.setItem('pacientes', JSON.stringify(pacientes.value));
};

const guardarPaciente = () => {
  if (paciente.id) {
    const { id } = paciente;
    const i = pacientes.value.findIndex(
      (pacienteState) => pacienteState.id === id
    );
    pacientes.value[i] = { ...paciente };
  } else {
    pacientes.value.push({
      ...paciente,
      id: uid(),
    });
  }

  //Reiniciar objeto
  paciente.nombreMascota = "";
  paciente.nombrePropietario = "";
  paciente.email = "";
  paciente.alta = "";
  paciente.sintomas = "";
  paciente.id = null;
};

const actualizarPaciente = (id) => {
  const pacienteEditar = pacientes.value.filter(
    (paciente) => paciente.id === id
  )[0];
  Object.assign(paciente, pacienteEditar);
};

const eliminarPaciente = (id) => {
  pacientes.value.filter((paciente) => paciente.id !== id);
};
</script>
