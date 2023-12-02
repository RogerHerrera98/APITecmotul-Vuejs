<script setup>
import { ref, defineProps } from 'vue';

const props = defineProps({
    msg: Object
});

const evaluacionFin = ref(false);

const variable1 = ref(0); // Valor inicial para Variable 1
const variable2 = ref(0); // Valor inicial para Variable 2


// Calcula los porcentajes basados en msg.listas y msg.faltantes
const calcularPorcentajes = () => {
    variable1.value = Math.round((props.msg.listas / (props.msg.listas + props.msg.faltantes)) * 100);
    variable2.value = Math.round((props.msg.faltantes / (props.msg.listas + props.msg.faltantes)) * 100);
    console.log("Porcentaje 1 " + props.msg.faltantes)
    console.log("Porcentaje 1 " + variable2)
};

// Llama a la función para calcular los porcentajes al inicio
calcularPorcentajes();

const evaluacionTerminal = () =>{
    if (variable1.value > 90) {
        evaluacionFin.value = true;
    }
}

evaluacionTerminal();
</script>

<template>
    <div class="max-w-md mx-auto">
        <div class="flex">
            <div class="w-full">
                <div class="mb-4">
                    <p>Usuarios Listos</p>
                    <div class="w-full flex flex-row ">
                        <img src="./img/cheque.png" alt="" class="w-10 h-10 ">
                        <div :style="{ 'width': variable1 + '%' }" class="bg-gradient-to-r from-emerald-500 from-10% to-emerald-900 to-100% h-10  "> </div>

                    </div>
                    <p class="px-2">{{ variable1 }} %</p>
                    <p class="text-xs"> Ya han respondido {{ msg.listas }} alumnos</p>
                </div>

                <div v-if="!evaluacionFin">
                    <div class="text-black">Usuarios faltantes </div>
                    <div class="w-full flex flex-row">
                        <img src="./img/bloquear.png" alt="" class="w-10 h-10">
                        <div :style="{ 'width': variable2 + '%' }" class="bg-gradient-to-r from-pink-500 from-10%  to-red-500 to-90% h-10"></div>
                    </div>
                    <p class="px-2">{{ variable2 }} %</p>
                    <p class="text-xs"> Faltan {{ msg.faltantes }} alumnos</p>
                </div>
                <div v-else class="flex justify-center items-center flex-col">
                    <img src="./img/comprobado.png" alt="" class="w-1/2">
                    <p>Se han respondido el 90% de las evaluaciones!!! </p>
                </div>
            </div>
        </div>
    </div>
</template>

