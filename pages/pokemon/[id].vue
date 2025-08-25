<template>
  <div v-if="pokemon">
    <h1>{{ capitalize(pokemon.name) }}</h1>
    <img :src="pokemon.image" :alt="pokemon.name" />
    <div class="types">
      <span
        v-for="t in pokemon.types"
        :key="t.type.name"
        :class="'type-' + t.type.name"
      >
        {{ capitalize(t.type.name) }}
      </span>
    </div>
    <p>Height: {{ pokemon.height }}</p>
    <p>Weight: {{ pokemon.weight }}</p>
    <p>Abilities: {{ abilities }}</p>


  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'
import { useRoute } from 'vue-router'
import axios from 'axios'

interface Pokemon {
  name: string
  image: string
  backImage: string
  height: number
  weight: number
  abilities: { ability: { name: string } }[]
  types: { type: { name: string } }[]
}


const route = useRoute()
const pokemon = ref<Pokemon | null>(null)

const fetchPokemon = async () => {
  const res = await axios.get(`https://pokeapi.co/api/v2/pokemon/${route.params.id}`)
  pokemon.value = {
    name: res.data.name,
    image: res.data.sprites.front_default,
    backImage: res.data.sprites.back_default, 
    height: res.data.height,
    weight: res.data.weight,
    abilities: res.data.abilities,
    types: res.data.types
  }
}

onMounted(fetchPokemon)

const capitalize = (str: string) => str.charAt(0).toUpperCase() + str.slice(1)

const abilities = computed(() =>
  pokemon.value
    ? pokemon.value.abilities.map(a => capitalize(a.ability.name)).join(', ')
    : ''
)
</script>

<style>
.types {
  margin-top: 0.5rem;
  display: flex;
  gap: 0.3rem;
}

.types span {
  padding: 0.2rem 0.4rem;
  border-radius: 4px;
  color: white;
  font-size: 0.7rem;
}

.type-fire { background-color: #f08030; }
.type-water { background-color: #6890f0; }
.type-grass { background-color: #78c850; }
.type-electric { background-color: #f8d030; }
.type-ice { background-color: #98d8d8; }
.type-fighting { background-color: #c03028; }
.type-poison { background-color: #a040a0; }
.type-bug { background-color: #A6B91A; }
.type-normal { background-color: #A8A77A; }
.type-ground { background-color: #E2BF65; }
.type-flying { background-color: #A98FF3; }
.type-psychic { background-color: #F95587; }
.type-rock { background-color: #B6A136; }
.type-ghost { background-color: #735797; }
.type-dragon { background-color: #6F35FC; }
.type-dark { background-color: #705746; }
.type-steel { background-color: #B7B7CE; }
.type-fairy { background-color: #D685AD; }

</style>
