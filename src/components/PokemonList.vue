<template>
  <h1>Pokémon App</h1>
  <loaderSpin v-if="isLoading"/>
  <div 
    class="div-pokemon" 
    v-for="(pokemon, index) in list" :key="index" 
    @click="selectPokemon(getPokemonId(pokemon.url))"
  >
    <img :src="getPokemonImage(getPokemonId(pokemon.url))">
    {{pokemon.name}}
  </div>
</template>

<script>
import { getPokemonList, getPokemonImageUrl } from "../service/pokemon-service.js"
import { onMounted, ref } from "vue"
import { useRouter } from "vue-router"
import LoaderSpin from "./LoaderSpin.vue"

export default {
  components: {
    LoaderSpin
  },
  setup() {
    const list = ref([])
    const router = useRouter()
    const isLoading = ref(false)

    onMounted(() => {
      isLoading.value = true
      getPokemonList().then(resp => {
        list.value = resp.results
      }).finally(() => {
        isLoading.value = false
      })
    })

    const getPokemonId = url => 
      url.replace("https://pokeapi.co/api/v2/pokemon/", "").replace("/", "")

    const getPokemonImage = id => getPokemonImageUrl(id)

    const selectPokemon = id => router.push({name: "pokemon", params: {id} })

    return { list, getPokemonId, getPokemonImage, selectPokemon, isLoading }
  }
}
</script>

<style scoped>
  h1 {
    text-align: center;
  }
  .div-pokemon {
    display: flex;
    align-items: center;
    border: 1px solid lightgray;
    border-radius: 0.2rem;
    margin: 0.5rem;
    cursor: pointer;
  }
</style>