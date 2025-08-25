<template>
  <div>
    <header>
      <h1>Pokédex</h1>
      <NuxtLink to="/">Home</NuxtLink>
    </header>

    <NuxtPage />

    <footer>
      <p>© 2025 Pokémon Info</p>
    </footer>
  </div>
</template>

<script setup lang="ts"></script>

<style>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
}

header a {
  color: #42b983;
  text-decoration: none;
}

footer {
  margin-top: 2rem;
  text-align: center;
  color: #666;
  font-size: 0.9rem;
}
</style>
