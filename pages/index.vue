<template>
  <div>
    <SearchBar v-model="search" placeholder="Search Pokémon..." />
    <!--Loop and display filtered pokemon in a grid-->
    <div class="pokemon-grid">
      <PokemonCard
        v-for="pokemon in filteredPokemon"
        :key="pokemon.id"
        :pokemon="pokemon"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'
import axios from 'axios'
import PokemonCard from '~/components/PokemonCard.vue'
import SearchBar from '~/components/SearchBar.vue'

interface Pokemon {
  id: number
  name: string
  image: string
}

const pokemonList = ref<Pokemon[]>([])
const search = ref('')

// API call via Axios
const fetchPokemon = async () => {
  const promises = Array.from({ length: 60 }, (_, i) =>
    axios.get(`https://pokeapi.co/api/v2/pokemon/${i + 1}`)
  )
  const results = await Promise.all(promises)
  pokemonList.value = results.map(res => ({
    id: res.data.id,
    name: res.data.name,
    image: res.data.sprites.front_default,
    backImage: res.data.sprites.back_default, // optional for hover
    types: res.data.types // Array of type objects
  }))
}

onMounted(fetchPokemon)

const filteredPokemon = computed(() =>
  pokemonList.value.filter(p =>
    p.name.toLowerCase().includes(search.value.toLowerCase())
  )
)
</script>

<style scoped>
.pokemon-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
  gap: 1rem;
  margin-top: 1rem;
}
</style>
