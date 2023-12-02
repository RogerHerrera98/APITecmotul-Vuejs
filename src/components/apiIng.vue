
  
<script setup>
import { ref, onMounted } from 'vue';
import iscTarjeta from './iscTarjeta.vue';
import ieTarjeta from './ieTarjeta.vue';
import ierTarjeta from './ierTarjeta.vue';
import iemTarjeta from './iemTarjeta.vue';
import iiTarjeta from './iiTarjeta.vue';

const info = ref(null);



const fetchData = async () => {
    try {
        const response = await fetch('https://sitmotul.com.mx/api/statusEvalIng');
        info.value = await response.json();

        console.log("Datos de las carreras:", info.value);
    } catch (error) {
        console.log('Error al obtener datos: ', error);
    }
};

onMounted(() => {
    fetchData();
});


</script>

<template>
    <div class="container mx-auto w-2/3 h-full shandow rounded-lg flex items-center justify-center bg-[#7dd3fc] flex-col px-3 py-4">
        <p class="font-sans text-4xl font-semibold py-4"> Evaluaciones por ingenieria</p>
        <div class="flex w-full justify-center items-center text-center ">
            <div class="w-1/3 m-2 p-4 bg-slate-50 font-sans text-lg  font-semibold rounded-lg transition ease-in-out delay-150  hover:-translate-y-1 hover:scale-110 hover:bg-slate-50 duration-300 ">
                <h2 class=" max-md:text-xs font-sans ">Sistemas Computacionales</h2>
                <img src="./img/ISC.png" alt="">
                <iscTarjeta v-if="info && info.ISC" :msg=info.ISC />
            </div>

            <div class="w-1/3 m-2 p-4 bg-slate-50 font-sans text-lg  font-semibold rounded-lg transition ease-in-out delay-150  hover:-translate-y-1 hover:scale-110 hover:bg-slate-50 duration-300">
                <h2>Electrónica</h2>
                <img src="./img/IE.png" alt="">
                <ieTarjeta v-if="info && info.IE" :msg=info.IE />
            </div>
            <div class="w-1/3 m-2 p-4 bg-slate-50 font-sans text-lg  font-semibold rounded-lg transition ease-in-out delay-150  hover:-translate-y-1 hover:scale-110 hover:bg-slate-50 duration-300">
                <h2>Energías Renovables</h2>
                <img src="./img/IER.png" alt="">
                <ierTarjeta v-if="info && info.IER" :msg=info.IER />
            </div>
        </div>
        <div class="flex w-full justify-center items-center text-center">
            <div class=" w-1/3 h-full m-2 p-4 bg-slate-50 font-sans text-lg  font-semibold rounded-lg transition ease-in-out delay-150  hover:-translate-y-1 hover:scale-110 hover:bg-slate-50 duration-300">
                <h2>Electromecánica </h2>
                <div class="flex justify-center items-center">
                    <img src="./img/IEM.png" alt="" class="w-36" >
                </div>
                
                <iemTarjeta v-if="info && info.IEM" :msg=info.IEM />
            </div>
            <div class="w-1/3 h-full m-2 p-4 bg-slate-50 font-sans text-lg  font-semibold rounded-lg transition ease-in-out delay-150  hover:-translate-y-1 hover:scale-110 hover:bg-slate-50 duration-300">
                <h2>Industrial</h2>
                <div class="flex justify-center items-center ">
                    <img src="./img/II.png" alt="" class="w-24">
                </div>
                <iiTarjeta v-if="info && info.II" :msg=info.II />
                
            </div>
        </div>
    </div>

        

   
</template>
