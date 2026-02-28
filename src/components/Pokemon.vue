<script setup>
import axios from 'axios' //libreria para peticiones http a la API
import { ref } from 'vue'; //variables reactivas
import { RouterLink } from 'vue-router'; //navegar entre rutas sin recargar la pag
import Paginacion from '@/components/Paginacion.vue'

const pokemons = ref([])  //variable reactiva, alamcena pokemons de la API

const postXpage = 10 //numero de post por pagina 10
const inicio = ref(0) //donde queremos iniciar, en 0
const fin = ref(postXpage) //indice del fin, donde finalizar


//avanzar a la sig pag
const next = () => {
  inicio.value = inicio.value + postXpage
  fin.value = fin.value + postXpage
}

//retroceder
const preview = () => {
  inicio.value -= postXpage
  fin.value -= postXpage

}

//llamada a la API
const getData = async () => {
    try {
        const { data } = await axios.get('https://pokeapi.co/api/v2/pokemon/?limit=10000') //peticion get a la API
        pokemons.value = data.results //guardar 
    } catch (error) {
        console.log('error')
    }
}
getData()
</script>

<template>
    <div class="min-h-screen bg-gray-100 p-8 "> <!-- cntenedor principal-->
        <h1 class="text-4xl font-bold text-yellow-500 mb-8">🔴 Pokémons</h1> <!-- titulo-->

        <div class="max-w-4xl mx-auto text-center">
            
            <ul class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-4">
                <li v-for="pokemon in pokemons.slice(inicio, fin)" :key="pokemon.name"> <!-- renderizado dianmico, por cada pokemon crea un LI-->
                    <RouterLink
                        :to="`/pokemon/${pokemon.name}`"
                        class="block bg-white rounded-xl shadow hover:shadow-md hover:scale-105 transition-all duration-200 p-4 text-center capitalize font-semibold text-gray-700 hover:text-yellow-500"
                    >
                        {{ pokemon.name }}
                    </RouterLink><!-- navegacion-->
                </li>
            </ul>

            <Paginacion  
                @next="next" 
                @preview="preview"
                :inicio="inicio"
                :fin="fin"
                :maxlength="pokemons.length"
            />
        </div>
    </div>
</template>