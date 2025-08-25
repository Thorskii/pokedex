<template>
  <NuxtLink 
    :to="`/pokemon/${pokemon.id}`" 
    class="card"
    :style="{ borderColor: typeColors[pokemon.types[0]?.type.name] || '#ccc' }"
  >
    <!-- Image container with hover swap -->
    <div class="img-container">
      <img :src="pokemon.image" :alt="pokemon.name" class="front" />
      <img :src="pokemon.animatedFront" :alt="pokemon.name + ' animated'" class="back" />
    </div>

    <p>{{ capitalize(pokemon.name) }}</p>

    <!-- Type badges -->
    <div class="types">
      <span 
        v-for="t in pokemon.types" 
        :key="t.type.name" 
        :class="'type-' + t.type.name"
      >
        {{ capitalize(t.type.name) }}
      </span>
    </div>
  </NuxtLink>
</template>

<script setup lang="ts">
import { ref } from 'vue'

defineProps<{
  pokemon: { 
    id: number
    name: string
    image: string
    backImage?: string
    animatedFront?: string
    types: { type: { name: string } }[]
  }
}>()

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

const capitalize = (str: string) => str.charAt(0).toUpperCase() + str.slice(1)
</script>

<style scoped>
.card {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-decoration: none;
  border: 2px solid;
  padding: 0.6rem;
  border-radius: 8px;
  transition: transform 0.3s;
  cursor: pointer;
  color: black;

  width: 100%;
  max-width: 320px;
  box-sizing: border-box;
}

.card:hover {
  transform: scale(1.05);
}

.img-container {
  width: 96px;           /* fixed size container */
  height: 96px;          /* fixed size container */
  position: relative;
  overflow: hidden;      /* ensures GIFs don’t overflow */
}

.img-container img {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  max-width: 100%;       /* scale GIFs down */
  max-height: 100%;      /* scale GIFs down */
}

.img-container img.back {
  opacity: 0;
  transition: opacity 0.3s ease;
}

.img-container:hover img.front {
  opacity: 0;
}

.img-container:hover img.back {
  opacity: 1;
}

p {
  margin-top: 0.5rem;
  font-size: 1rem;
  text-align: center;
  word-break: break-word;
}

.types {
  margin-top: 0.5rem;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 0.3rem;
}

.types span {
  padding: 0.25rem 0.5rem;
  border-radius: 15px;
  color: white;
  font-size: 0.75rem;
  text-align: center;
  white-space: nowrap;
}

/* Responsive adjustments */
@media (max-width: 768px) {
  .card {
    max-width: 250px;
    padding: 0.5rem;
  }

  .img-container {
    width: 80px;
    height: 80px;
  }

  p {
    font-size: 0.9rem;
  }

  .types span {
    font-size: 0.65rem;
    padding: 0.2rem 0.4rem;
  }
}

@media (max-width: 480px) {
  .card {
    max-width: 120px;
    padding: 0.4rem;
  }

  .img-container {
    width: 70px;
    height: 70px;
  }

  p {
    font-size: 0.8rem;
  }

  .types {
    gap: 0.2rem;
  }

  .types span {
    font-size: 0.6rem;
    padding: 0.15rem 0.3rem;
  }
}

/* Type colors */
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
