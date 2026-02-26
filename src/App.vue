<script setup>
import { RouterView, RouterLink } from 'vue-router';
</script>

<template>
  <nav class="bg-gray-900 px-6 py-3 flex items-center gap-6 shadow-md sticky top-0 z-50">
    <span class="text-white font-bold text-lg tracking-widest uppercase">Pokédex</span>
    <div class="flex gap-1 ml-auto">
      <RouterLink
        to="/"
        class="text-gray-400 hover:text-white hover:bg-gray-700 px-4 py-2 rounded-lg text-sm font-medium transition"
        active-class="text-white bg-gray-700"
      >Home</RouterLink>
      <RouterLink
        to="/about"
        class="text-gray-400 hover:text-white hover:bg-gray-700 px-4 py-2 rounded-lg text-sm font-medium transition"
        active-class="text-white bg-gray-700"
      >About</RouterLink>
      <RouterLink
        to="/login"
        class="text-gray-400 hover:text-white hover:bg-gray-700 px-4 py-2 rounded-lg text-sm font-medium transition"
        active-class="text-white bg-gray-700"
      >Login</RouterLink>
      <RouterLink
        to="/pokemon"
        class="text-gray-400 hover:text-white hover:bg-gray-700 px-4 py-2 rounded-lg text-sm font-medium transition"
        active-class="text-white bg-gray-700"
      >Pokéapi</RouterLink>
    </div>
  </nav>

  <RouterView />
</template>