<script setup>
import axios from 'axios'
import { ref } from 'vue';
import { useRoute, useRouter } from 'vue-router';

const route = useRoute() //acceso a los parametros URL actual
const router = useRouter() //navegar entre rutas
const pokemon = ref(null) //VAR reactiva, guarda respuesta de la API, es decir datos de los pokemons

//funcion para volver 
const back = () => {
    router.push('/pokemon')
}

//peticion a la API
const getData = async () => {
    try {
        const { data } = await axios.get(`https://pokeapi.co/api/v2/pokemon/${route.params.name}`)
        pokemon.value = data //guardar
    } catch (error) {
        console.log(error)
    }
}

getData() //ejecucion cuando el componente carga
</script>

<template>
    
    <div class="min-h-screen bg-gray-100 flex items-center justify-center p-8">
        <div v-if="pokemon" class="bg-white rounded-2xl shadow-lg p-8 max-w-sm w-full text-center"> <!--si llegan los datos-->
            <!--usar imagen frontal por defecto, nombre y id-->
            <img 
                :src="pokemon.sprites.front_default" 
                :alt="pokemon.name"
                class="w-40 h-40 mx-auto"
            />

            <h1 class="text-3xl font-bold capitalize text-yellow-500 mb-2">{{ pokemon.name }}</h1>
            <p class="text-gray-500 mb-4">ID: #{{ pokemon.id }}</p>

            <div class="flex justify-center gap-2 mb-4">
                <!--tipo de pokemon, recorrer-->
                <span 
                    v-for="type in pokemon.types" 
                    :key="type.type.name"
                    class="bg-yellow-100 text-yellow-700 text-sm font-semibold px-3 py-1 rounded-full capitalize"
                >
                    {{ type.type.name }}
                </span>
            </div>
                <!--obten en en m y kg al dividir entre 10-->
            <div class="grid grid-cols-2 gap-3 text-sm text-gray-600 mb-6">
                <div class="bg-gray-50 rounded-xl p-3">
                    <p class="font-semibold text-gray-800">Altura</p>
                    <p>{{ pokemon.height / 10 }} m</p>
                </div>
                <div class="bg-gray-50 rounded-xl p-3">
                    <p class="font-semibold text-gray-800">Peso</p>
                    <p>{{ pokemon.weight / 10 }} kg</p>
                </div>
            </div>

            <button 
                @click="back"
                class="bg-yellow-400 hover:bg-yellow-500 text-white font-bold py-2 px-6 rounded-full transition-colors"
            >
                ← Volver
            </button>
        </div>

        <div v-else class="text-gray-500 text-xl">⚙️Cargando...</div> <!--si no muestra cargando-->
    </div>
</template>