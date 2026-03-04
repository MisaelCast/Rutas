<script setup>
import axios from 'axios';
import { ref, onMounted } from 'vue';
import { useRoute, useRouter } from 'vue-router';

const route = useRoute();
const router = useRouter();
const pokemon = ref(null);

// Función para obtener los datos del Pokémon usando su nombre desde la ruta
const getdata = async () => {
    try {
        const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${route.params.name}`);
        pokemon.value = response.data;
        console.log(pokemon.value);
    } catch (error) {
        console.error("Hubo un error al llamar a la API:", error);
    }
};

onMounted(() => {
    getdata();
});
</script>

<template>
    <div v-if="pokemon" class="p-6 max-w-md mx-auto">

        <button @click="router.back()" class="mb-4 text-sm text-gray-500 hover:text-black">← Volver</button>

        <div class="bg-gray-100 rounded-2xl p-6">

            <h1 class="text-2xl font-bold capitalize mb-3">{{ pokemon.name }}</h1>

            <img
                
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokemon.id}.png`"
                :alt="pokemon.name"
                class="w-32 h-32 object-contain mx-auto"
            />

            <div class="grid grid-cols-2 gap-3 mt-4 text-sm">
                <div class="bg-white rounded-xl p-3">
                    <p class="text-gray-400 text-xs">Altura</p>
                    <p class="font-semibold">{{ pokemon.height / 10 }} m</p>
                </div>
                <div class="bg-white rounded-xl p-3">
                    <p class="text-gray-400 text-xs">Peso</p>
                    <p class="font-semibold">{{ pokemon.weight / 10 }} kg</p>
                </div>
                <div class="bg-white rounded-xl p-3">
                    <p class="text-gray-400 text-xs">Tipos</p>
                    <p class="font-semibold capitalize">{{ pokemon.types.map(t => t.type.name).join(', ') }}</p>
                </div>
                <div class="bg-white rounded-xl p-3">
                    <p class="text-gray-400 text-xs">Habilidades</p>
                    <p class="font-semibold capitalize">{{ pokemon.abilities.map(a => a.ability.name).join(', ') }}</p>
                </div>
            </div>

            <p class="font-bold mt-5 mb-2">Estadísticas</p>
            <ul class="space-y-2">
                <li v-for="stat in pokemon.stats" :key="stat.stat.name" class="flex items-center gap-2 text-sm">
    <span class="w-28 text-gray-500 capitalize">{{ stat.stat.name }}</span>
    <span class="w-6 font-semibold text-right">{{ stat.base_stat }}</span>
    <div class="flex-1 bg-gray-300 rounded-full h-2">
        <div
            class="bg-gray-700 h-2 rounded-full"
            :style="{ width: `${stat.base_stat}%` }"
        ></div>
    </div>
</li>
            </ul>

        </div>
    </div>

    <p v-else class="p-6 text-gray-500">Cargando...</p>
</template>