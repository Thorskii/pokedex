<template>
  <div>
    <!-- Search bar with type support -->
    <SearchBar v-model="search" :types="allTypes" placeholder="Search Pokémon..." />

    <!-- Loop and display filtered pokemon in a grid -->
    <div class="pokemon-grid">
      <PokemonCard v-for="pokemon in filteredPokemon" :key="pokemon.id" :pokemon="pokemon" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, watch } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import axios from 'axios'
import PokemonCard from '~/components/PokemonCard.vue'
import SearchBar from '~/components/SearchBar.vue'

interface Pokemon {
  id: number
  name: string
  image: string
  backImage: string
  animatedFront?: string
  types: { type: { name: string } }[]
}

const pokemonList = ref<Pokemon[]>([])

const search = ref({ name: '', type: '' })

const allTypes = ref<string[]>([])

const route = useRoute()

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
    backImage: res.data.sprites.back_default,
    animatedFront: res.data.sprites.versions['generation-v']['black-white'].animated.front_default,
    types: res.data.types
  }))

  // Extract unique types
  const typesSet = new Set<string>()
  pokemonList.value.forEach(p =>
    p.types.forEach(t => typesSet.add(t.type.name))
  )
  allTypes.value = Array.from(typesSet)
}

onMounted(() => {
  fetchPokemon()
  if (route.query.type) {
    search.value.type = String(route.query.type)
  }
})

// Route listener
watch(
  () => route.query.type,
  (newType) => {
    search.value.type = newType ? String(newType) : ''
  }
)

// Filter by name and type
const filteredPokemon = computed(() =>
  pokemonList.value.filter(p =>
    p.name.toLowerCase().includes(search.value.name.toLowerCase()) &&
    (search.value.type === '' || p.types.some(t => t.type.name === search.value.type))
  )
)
</script>


<style scoped>
.pokemon-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
  gap: 1rem;
  margin-top: 1rem;
  padding: 1rem;
  background-image: url("assets/container_bg.png");
}

@media (max-width: 768px) {
  .pokemon-grid {
    grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
  }
}

@media (max-width: 480px) {
  .pokemon-grid {
    grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
  }
}
</style>
