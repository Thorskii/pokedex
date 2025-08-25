<template>
  <NuxtLink :to="`/pokemon/${pokemon.id}`" class="card">
    
    <!-- Show back sprite container -->
    <div 
      class="img-container" 
      @mouseenter="showBack = true" 
      @mouseleave="showBack = false"
    >
      <img :src="showBack ? pokemon.backImage : pokemon.image" :alt="pokemon.name" />
    </div>

    <p>{{ capitalize(pokemon.name) }}</p>

    <!-- Type badges -->
    <div class="types">
      <span v-for="t in pokemon.types" :key="t.type.name" :class="'type-' + t.type.name">
        {{ capitalize(t.type.name) }}
      </span>
    </div>
  </NuxtLink>
</template>

<script setup lang="ts">
import { ref } from 'vue'
defineProps<{
  pokemon: { id: number; name: string; image: string; backImage: string; types: any[] }
}>()

const showBack = ref(false)

const capitalize = (str: string) => str.charAt(0).toUpperCase() + str.slice(1)
</script>

<style scoped>
.card {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-decoration: none;
  border: 1px solid #ccc;
  padding: 0.5rem;
  border-radius: 8px;
  transition: transform 0.3s;
  cursor: pointer;
  color: black;
}

.card:hover {
  transform: scale(1.05);
}

.img-container {
  width: 80px;
  height: 80px;
}

img {
  width: 100%;
  height: 100%;
}

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
