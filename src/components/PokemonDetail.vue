<script setup>
import axios from 'axios';
import { ref, onMounted } from 'vue';
import { useRoute, useRouter } from 'vue-router';

const route = useRoute();
const router = useRouter();
const pokemon = ref(null);
const loading = ref(true);

const statNames = {
  'hp': 'HP', 'attack': 'ATK', 'defense': 'DEF',
  'special-attack': 'Sp.ATK', 'special-defense': 'Sp.DEF', 'speed': 'VEL',
};

onMounted(async () => {
  try {
    const { data } = await axios.get(`https://pokeapi.co/api/v2/pokemon/${route.params.name}`);
    pokemon.value = data;
  } catch (e) {
    console.error(e);
  } finally {
    loading.value = false;
  }
});
</script>

<template>
  <div class="p-6 max-w-md mx-auto">
    <button @click="router.back()" class="mb-4 text-sm text-gray-500 hover:text-black">
      ← Volver
    </button>

    <p v-if="loading" class="text-gray-500">Cargando...</p>

    <div v-else-if="pokemon" class="bg-gray-100 rounded-2xl p-6">
      <div class="flex flex-col items-center mb-4">
        <img
          :src="pokemon.sprites.other['official-artwork'].front_default"
          :alt="pokemon.name"
          class="w-40 h-40 object-contain"
        />
        <h1 class="text-2xl font-bold capitalize mt-2">{{ pokemon.name }}</h1>
        <p class="text-gray-400 text-sm">#{{ String(pokemon.id).padStart(3, '0') }}</p>

        <div class="flex gap-2 mt-2">
          <span
            v-for="t in pokemon.types"
            :key="t.type.name"
            class="text-xs bg-gray-300 rounded-full px-3 py-1 capitalize font-semibold"
          >
            {{ t.type.name }}
          </span>
        </div>
      </div>

      <!-- Info básica -->
      <div class="grid grid-cols-2 gap-3 mb-4 text-sm">
        <div class="bg-white rounded-xl p-3">
          <p class="text-gray-400 text-xs">Altura</p>
          <p class="font-semibold">{{ (pokemon.height / 10).toFixed(1) }} m</p>
        </div>
        <div class="bg-white rounded-xl p-3">
          <p class="text-gray-400 text-xs">Peso</p>
          <p class="font-semibold">{{ (pokemon.weight / 10).toFixed(1) }} kg</p>
        </div>
        <div class="bg-white rounded-xl p-3 col-span-2">
          <p class="text-gray-400 text-xs">Habilidades</p>
          <p class="font-semibold capitalize">
            {{ pokemon.abilities.map(a => a.ability.name).join(', ') }}
          </p>
        </div>
      </div>

      <!-- Stats -->
      <h2 class="font-bold mb-2">Estadísticas</h2>
      <div class="space-y-2">
        <div v-for="stat in pokemon.stats" :key="stat.stat.name" class="flex items-center gap-2 text-sm">
          <span class="w-16 text-gray-500 text-xs">{{ statNames[stat.stat.name] || stat.stat.name }}</span>
          <span class="w-8 font-semibold text-right">{{ stat.base_stat }}</span>
          <div class="flex-1 bg-gray-300 rounded-full h-2">
            <div
              class="bg-gray-700 h-2 rounded-full"
              :style="{ width: `${Math.min(stat.base_stat / 255 * 100, 100)}%` }"
            ></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>