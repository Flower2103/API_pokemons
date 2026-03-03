<script setup>
import axios from 'axios' //libreria para peticiones http a la API
import { ref, watch} from 'vue'; //variables reactivas
import { RouterLink } from 'vue-router'; //navegar entre rutas sin recargar la pag
import Paginacion from '@/components/Paginacion.vue'

const pokemons = ref([])  //variable reactiva, alamcena pokemons de la API
const paginaActual = ref([])

const postXpage = 12 //numero de post por pagina 10
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

// Carga los detalles solo de los pokemons visibles
const cargarPagina = async () => {
  const slice = pokemons.value.slice(inicio.value, fin.value)
  const detalles = await Promise.all(
    slice.map(p => axios.get(p.url).then(r => ({
      name: r.data.name,
      imagen: r.data.sprites.front_default,
      tipos: r.data.types.map(t => t.type.name)
    })))
  )
  paginaActual.value = detalles
}

// Cuando cambia inicio, recarga la pagina
watch(inicio, cargarPagina)



//llamada a la API
const getData = async () => {
    try {
        const { data } = await axios.get('https://pokeapi.co/api/v2/pokemon/?limit=10000') //peticion get a la API
        pokemons.value = data.results //guardar 
        cargarPagina()
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
            <li v-for="pokemon in paginaActual" :key="pokemon.name">
            <RouterLink
            :to="`/pokemon/${pokemon.name}`"
            class="block bg-white rounded-xl shadow hover:shadow-md hover:scale-105 transition-all duration-200 p-4 text-center capitalize font-semibold text-gray-700 hover:text-yellow-500"
            >
            <img :src="pokemon.imagen" :alt="pokemon.name" class="mx-auto w-20 h-20" />
            <p>{{ pokemon.name }}</p>
                <div class="flex justify-center gap-1 mt-1">
                    <span
                    v-for="tipo in pokemon.tipos"
                    :key="tipo"
                    class="text-xs bg-yellow-100 text-yellow-700 rounded-full px-2 py-0.5"
                    >
                    {{ tipo }}
                    </span>
                </div>
            </RouterLink>
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