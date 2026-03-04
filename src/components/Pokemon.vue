<script setup>
import axios from 'axios';
import { ref, onMounted } from 'vue';
import { RouterLink } from 'vue-router';

//agregar paginacion 
const limit = 20; // Número de pokemones por página
const offset = ref(0); // Desplazamiento inicial


const pokemons = ref([]);

const getdata = async () => {
    try {

        // Axios ya devuelve la respuesta en la propiedad 'data'
        const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/?limit=${limit}&offset=${offset.value}`);
      
        // La PokéAPI entrega los resultados directamente en .results
        pokemons.value = response.data.results;
        
        console.log(pokemons.value);
    } catch (error) {
        console.error("Hubo un error al llamar a la API:", error);
    }
};

// Es una buena práctica llamar a la función cuando el componente se monta
onMounted(() => {
    getdata();
});
</script>

<template>
  <div class="p-6">
    <ul class="grid grid-cols-4 gap-3">
      <li v-for="(pokemon, index) in pokemons" :key="pokemon.name">
        <RouterLink
          :to="`/pokemon/${pokemon.name}`"
          class="bg-gray-100 hover:bg-gray-200 rounded-xl p-3 flex flex-col items-center transition"
        >
          <img
            :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${offset + index + 1}.png`"
            :alt="pokemon.name"
            class="w-16 h-16 object-contain"
          />
          <span class="text-sm capitalize mt-1">{{ pokemon.name }}</span>
        </RouterLink>
      </li>
    </ul>

    <div class="flex items-center justify-center gap-4 mt-6">
      <button
        @click="offset -= limit; getdata()"
        :disabled="offset === 0"
        class="bg-gray-800 text-white px-4 py-2 rounded-lg hover:bg-gray-700 transition disabled:opacity-30"
      >Anterior</button>
      <span class="text-sm text-gray-500">Página {{ offset / limit + 1 }}</span>
      <button
        @click="offset += limit; getdata()"
        class="bg-gray-800 text-white px-4 py-2 rounded-lg hover:bg-gray-700 transition"
      >Siguiente</button>
    </div>
  </div>
</template>