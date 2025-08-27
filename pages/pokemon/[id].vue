<template>
  <div v-if="pokemon" class="detail-page">
    <div class="card large-card" :style="{ borderColor: typeColors[pokemon.types[0]?.type.name] || '#ccc' }">
      <div class="sprite-container">
        <img :src="pokemon.animatedFront" :alt="pokemon.name + ' front'" class="pokemon-image front" />
        <img :src="pokemon.animatedBack" :alt="pokemon.name + ' back'" class="pokemon-image back" />
      </div>

      <h1>{{ capitalize(pokemon.name) }}</h1>

      <div class="types">
        <NuxtLink v-for="t in pokemon.types" :key="t.type.name" :to="{ path: '/', query: { type: t.type.name } }"
          :class="['type-badge', 'type-' + t.type.name]">
          {{ capitalize(t.type.name) }}
        </NuxtLink>
      </div>

      <div class="stats">
        <h3>Information</h3>
        <p><b>Height:&nbsp;</b>{{ pokemon.height }} m</p>
        <p><b>Weight:&nbsp;</b>{{ pokemon.weight }} kg</p>
        <p><b>Abilities:&nbsp;</b>{{ abilities }}</p>

        <div class="base-stats">
          <h3>Base Stats</h3>
          <div v-for="s in pokemon.stats" :key="s.stat.name" class="stat-row">
            <span class="stat-name">{{ capitalize(s.stat.name) }}</span>
            <div class="stat-bar">
              <div class="stat-fill"
                :style="{ width: (s.base_stat / 255) * 100 + '%', backgroundColor: typeColors[pokemon.types[0]?.type.name] || '#333' }">
              </div>
            </div>
            <span class="stat-value">{{ s.base_stat }}</span>
          </div>
        </div>
      </div>
    </div>
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
  animatedFront?: string
  animatedBack?: string
  height: number
  weight: number
  abilities: { ability: { name: string } }[]
  types: { type: { name: string } }[]
  stats: { base_stat: number; stat: { name: string } }[]
}

// Color map
const typeColors: Record<string, string> = {
  fire: '#f08030',
  water: '#6890f0',
  grass: '#78c850',
  electric: '#f8d030',
  ice: '#98d8d8',
  fighting: '#c03028',
  poison: '#a040a0',
  bug: '#A6B91A',
  normal: '#A8A77A',
  ground: '#E2BF65',
  flying: '#A98FF3',
  psychic: '#F95587',
  rock: '#B6A136',
  ghost: '#735797',
  dragon: '#6F35FC',
  dark: '#705746',
  steel: '#B7B7CE',
  fairy: '#D685AD',
}

const route = useRoute()
const pokemon = ref<Pokemon | null>(null)

// Axios API call 
const fetchPokemon = async () => {
  const res = await axios.get(`https://pokeapi.co/api/v2/pokemon/${route.params.id}`)
  pokemon.value = {
    name: res.data.name,
    image: res.data.sprites.front_default,
    backImage: res.data.sprites.back_default,
    animatedFront: res.data.sprites.versions['generation-v']['black-white'].animated.front_default,
    animatedBack: res.data.sprites.versions['generation-v']['black-white'].animated.back_default,
    height: (res.data.height / 10),
    weight: (res.data.weight / 10),
    abilities: res.data.abilities,
    types: res.data.types,
    stats: res.data.stats
  }
}

onMounted(fetchPokemon)

const capitalize = (str: string) => str.charAt(0).toUpperCase() + str.slice(1)

// Pokemon abilites set to capitalized, comma delimiting string
const abilities = computed(() =>
  pokemon.value
    ? pokemon.value.abilities.map(a => capitalize(a.ability.name)).join(', ')
    : ''
)
</script>

<style>
.types {
  margin: 0.5rem;
  display: flex;
  gap: 5px;
}

.type-badge {
  padding: 0.4rem 0.8rem;
  border-radius: 15px;
  color: white;
  font-size: 1rem;
  text-decoration: none;
  transition: transform 0.2s ease, opacity 0.2s ease;
}

.type-badge:hover {
  transform: scale(1.05);
  opacity: 0.9;
}

.sprite-container {
  position: relative;
  width: 150px;
  height: 150px;
  margin: 1rem auto;
}

.pokemon-image {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 80%;
  height: 80%;
  object-fit: contain;
  transform: translate(-50%, -50%);
  transition: opacity 0.3s ease;
}

/* back sprite fades in on hover */
.pokemon-image.back {
  opacity: 0;
}

.sprite-container:hover .pokemon-image.front {
  opacity: 0;
}

.sprite-container:hover .pokemon-image.back {
  opacity: 1;
}

@media (min-width: 768px) {
  .sprite-container {
    width: 200px;
    height: 200px;
  }
}

@media (min-width: 1200px) {
  .sprite-container {
    width: 250px;
    height: 250px;
  }
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

.detail-page {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 70vh;
  background-image: url("assets/Pokemonbg.png")
}

.card {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-decoration: none;
  border: 4px solid;
  padding: 2rem;
  border-radius: 16px;
  background: white;
  color: black;
  box-shadow: 5px 5px 10px rgba(0, 0, 0, 0.5);

}

.large-card {
  width: 500px;
  max-width: 90%;
  padding: 16px;
}

.img-container {
  width: 200px;
  height: 200px;
  margin-bottom: 1rem;
}

.img-container img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.stats {
  margin-top: 10px;
  justify-content: center;
  width: 100%;
  border: 2px solid rgba(0, 0, 0, 0.048);
  border-radius: 15px;
  background-color: rgba(0, 0, 0, 0.048);
  
}

.stats p, .stats h3 {
  margin: 6px 10px;
}

.base-stats {
  margin-top: 1rem;
}

.base-stats h3 {
  margin: 10px
}

.stat-row {
  display: flex;
  align-items: center;
  margin: 6px 10px;
  gap: 8px;
}

.stat-name {
  width: 80px;
  text-transform: capitalize;
}

.stat-bar {
  flex: 1;
  height: 10px;
  background: #eee;
  border-radius: 5px;
  overflow: hidden;
}

.stat-fill {
  height: 100%;
  border-radius: 5px;
}

.stat-value {
  width: 30px;
  text-align: right;
  font-weight: bold;
}
</style>
