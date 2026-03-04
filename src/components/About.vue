<script setup>
import { RouterLink } from 'vue-router';
</script>

<template>
    <div class="min-h-screen bg-gray-900 flex items-center justify-center">
        <div class="text-center text-white max-w-md px-6">
            <h1 class="text-4xl font-bold mb-4">Sobre este proyecto</h1>
            <p class="text-gray-400 mb-6">
                Esta es una Pokédex construida con Vue 3, Vue Router, Axios y Tailwind CSS.
                Los datos se obtienen en tiempo real desde la PokéAPI.
            </p>
            <RouterLink
                to="/pokemon"
                class="bg-white text-gray-900 px-6 py-2 rounded-lg font-semibold hover:bg-gray-200 transition"
            >
                Ver Pokédex →
            </RouterLink>
        </div>
    </div>
</template>