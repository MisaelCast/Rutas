<script setup>
import { ref, onMounted } from 'vue';

const texto = "Bienvenido a la Pokédex";
const display = ref("");

onMounted(() => {
    let i = 0;
    const intervalo = setInterval(() => {
        display.value += texto[i];
        i++;
        if (i === texto.length) clearInterval(intervalo);
    }, 100);
});
</script>

<template>
<div class="min-h-screen bg-gray-900 flex items-center justify-center">
        <h1 class="text-white text-4xl font-bold tracking-widest">
            {{ display }}
        </h1>
    </div>
</template>