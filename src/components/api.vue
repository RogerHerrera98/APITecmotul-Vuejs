<script setup>
import { ref, onMounted, computed } from 'vue';
import dayjs from 'dayjs';
import 'dayjs/locale/es'; // Importa el idioma español

dayjs.locale('es'); // Establece el idioma español

const info = ref({
  periodo: "",
  alTotal: "",
  alEvaluados: "",
  inicio: "",
  fin: "",
});
//Se creo una variable para el control del tamaño de la panatalla
const isPantallaPeque = ref(window.innerWidth >= 768);

const formatoFechaInicio = computed(() => {
  return dayjs(info.value.inicio).format('dddd, DD [de] MMMM [del] YYYY');
});

const formatoFechaFin = computed(() => {
  return dayjs(info.value.fin).format('dddd, DD [de] MMMM [del] YYYY');
});

const diferenciaHoras = computed(() => {
  const now = dayjs();
  const finDia = dayjs(info.value.fin);

  // Obtener la diferencia en horas (asegurarse de que sea positiva)
  const diferenciaHora = Math.max(0, finDia.diff(now, 'hour'));

  return diferenciaHora;
});

const fechaActual = computed(() => {
  return dayjs().format('dddd, DD [de] MMMM [del] YYYY');
});

const fetchData = async () => {
  try {
    const response = await fetch('https://sitmotul.com.mx/api/statusEval');
    const data = await response.json();
    info.value = data;
  } catch (error) {
    console.log('Error al obtener datos: ', error);
  }
};

onMounted(() => {
  fetchData();
});


const calcularPorcentaje = computed(() => {
  if (info.value && info.value.alTotal && info.value.alEvaluados) {
    const porcentaje = (info.value.alEvaluados / info.value.alTotal) * 100;
    return Math.round(porcentaje);
  } else {
    return 0;
  }
});

const calcularPorcentajeFaltantes = computed(() => {
  if (info.value && info.value.alTotal && info.value.alEvaluados) {
    const porcentaje = ((info.value.alTotal - info.value.alEvaluados) / info.value.alTotal) * 100;
    return Math.round(porcentaje);
  } else {
    return 0;
  }
});

const calcularColorAnillo = computed(() => {
  // Calcular el color y la apertura del anillo dinámicamente
  const colorIzquierdo = '#dc2626';  // Color izquierdo (rojo)
  const colorDerecho = '#15803d';  // Color derecho (verde)
  const apertura = 360 - (calcularPorcentaje.value / 100) * 360;  // Calcular la apertura (en grados)

  // Crear el gradiente cónico con la apertura dinámica
  return `conic-gradient(${colorIzquierdo} 0deg ${apertura}deg, 1deg, ${colorDerecho} 180deg 360deg)`;
});


</script>


<template>
  <div class="container w-2/3 justify-center items-center mx-auto ">
    <div class="text-center mb-8 py-4">
      <p v-if="info && info.periodo" class="font-sans text-lg  font-semibold ">Evaluación tutor {{ info.periodo }} <br> al
        {{ fechaActual }}</p>

      <div class="bg-[#f59e0b] rounded shadow h-full mb-8 py-4 ">

        <p class="font-sans font-semibold text-lg">Del {{ formatoFechaInicio }} </p>



        <div class="flex flex-row justify-center items-center px-2">
          <p v-if="info && info.fin" class="font-sans font-semibold px-2 text-lg">Faltan </p>
          <img src="./img/reloj.png" alt="" class="w-8 h-8">

        </div>
        <span class="font-sans  text-4xl font-semibold ">{{ diferenciaHoras }} hrs.</span>
        <p class="font-sans font-semibold text-lg">Al {{ formatoFechaFin }}</p>
      </div>



      <!-- ----------------------------------------------------------- -->



      <div v-if="info && info.fin" class="flex items-center justify-between py-4 px-2 bg-[#0ea5e9] rounded-lg shandown w-full h-full">
        <div class="flex flex-col items-center justify-center h-full w-1/3 ">
          <p class="font-sans text-black font-semibold text-md max-sm:text-sm">Porcentaje de Personas Evaluadas:</p>

          <div class="flex flex-row">
            <p v-if="info && info.alTotal" class="font-sans px-2 text-sky-200 text-5xl max-sm:text-2xl max-sm:px-0 font-bold "> {{
              calcularPorcentaje }} %
            </p>
            <img v-if="isPantallaPeque" src="./img/lista-de-verificacion.png" alt="evaluaciones completas"
              class=" w-16 h-16 ">

          </div>
          <span v-if="info && info.alEvaluados" class="font-sans text-sky-200 text-lg font-bold max-sm:text-sm">{{ info.alEvaluados
          }} de {{ info.alTotal }}</span>

        </div>

        <div class="relative  w-1/3 h-full p-12 max-sm:p-6 ">
          <div class="absolute inset-0 flex items-center justify-center rounded-bl-full anillo"
            :style="{ background: calcularColorAnillo }"></div>
        </div>

        <div class="relative w-1/3 h-full">
          <p class="font-sans text-black font-semibold text-md max-sm:text-sm">Porcentaje de Personas Faltantes</p>
         <div class="flex flex-row ">
          <img v-if="isPantallaPeque" src="./img/error.png" alt="evaluaciones faltantes" class="w-16 h-16">
          <p class="font-sans px-2 text-sky-200 text-5xl font-bold max-sm:text-2xl max-sm:px-0">{{ calcularPorcentajeFaltantes }} %</p>
          
         </div>
          <span class="font-sans text-sky-200 text-lg font-bold max-sm:text-sm">Faltan {{ info.alTotal - info.alEvaluados }}</span>
          

        </div>
      </div>


    </div>





  </div>
</template>

<style scoped>
.anillo {
  border-radius: 50%;
  clip-path: circle();
  /* Asegura que el anillo tenga forma circular */
  border-color: rgb(0 0 0);
  border-style: solid;
}
</style>