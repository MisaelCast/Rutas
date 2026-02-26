<script setup>
import axios from 'axios';
import { ref, onMounted } from 'vue';
import { RouterLink } from 'vue-router';

const pokemons = ref([]);
const loading = ref(true);
const loadingMore = ref(false);
const offset = ref(0);
const limit = 20;
const total = ref(0);

const getId = (url) => url.split('/').filter(Boolean).pop();
const padId = (id) => String(id).padStart(3, '0');
const getImg = (url) => {
  const id = getId(url);
  return `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${id}.png`;
};

const fetchPokemons = async () => {
  try {
    const { data } = await axios.get(`https://pokeapi.co/api/v2/pokemon?limit=${limit}&offset=${offset.value}`);
    total.value = data.count;
    pokemons.value = [...pokemons.value, ...data.results];
    offset.value += limit;
  } catch (error) {
    console.error("Error:", error);
  } finally {
    loading.value = false;
    loadingMore.value = false;
  }
};

const loadMore = () => {
  loadingMore.value = true;
  fetchPokemons();
};

onMounted(() => fetchPokemons());
</script>

<template>
  <div class="p-6">
    <h1 class="text-3xl font-bold mb-1">Pokédex</h1>
    <p class="text-gray-400 text-sm mb-6">{{ pokemons.length }} de {{ total }} pokémons</p>

    <p v-if="loading" class="text-gray-500">Cargando...</p>

    <div v-else>
      <div class="grid grid-cols-2 sm:grid-cols-4 md:grid-cols-5 gap-4">
        <RouterLink
          v-for="pokemon in pokemons"
          :key="pokemon.name"
          :to="`/pokemon/${pokemon.name}`"
          class="bg-gray-100 rounded-xl p-3 flex flex-col items-center hover:bg-gray-200 transition"
        >
          <img :src="getImg(pokemon.url)" :alt="pokemon.name" class="w-20 h-20 object-contain" loading="lazy" />
          <span class="text-xs text-gray-400">#{{ padId(getId(pokemon.url)) }}</span>
          <span class="text-sm font-semibold capitalize">{{ pokemon.name }}</span>
        </RouterLink>
      </div>

      <div v-if="offset < total" class="flex justify-center mt-8">
        <button
          @click="loadMore"
          :disabled="loadingMore"
          class="bg-gray-800 text-white px-6 py-2 rounded-lg hover:bg-gray-700 transition disabled:opacity-50"
        >
          {{ loadingMore ? 'Cargando...' : 'Cargar más' }}
        </button>
      </div>
    </div>
  </div>
</template>