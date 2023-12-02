<script setup>

import apiVue from './components/api.vue';
import apiIng from './components/apiIng.vue';
</script>

<template>
  <nav class="bg-blue-500 p-4 flex items-center justify-between">
    <div class="flex items-center">
      <!-- Logo -->
      <img src="./components/img/logo.png" alt="Logo" class="w-10 h-10 mr-2">
      <!-- Texto -->
      <p class=" text-lg font-bold">SITMOTUL</p>
    </div>
  </nav>

      <apiVue></apiVue>
      <apiIng></apiIng>

</template>
