<template>
  <div>
    <header>
      <NuxtLink to="/" class="logo-link">
        <img src="assets/pokeball.png" alt="Pokéball" class="pokeball" />

        <h1>Pokédex</h1>

      </NuxtLink>
    </header>

    <div class="body">
      <NuxtPage />
    </div>
      

    <footer>
      <p>© 2025 Austin Schroeder's Pokédex</p>
    </footer>
  </div>
</template>

<script setup lang="ts"></script>

<style>
* {
  font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
}
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  padding-bottom: 0;
}

header a {
  text-decoration: none;
  color: black;
}

.logo-link {
  display: flex;
  align-items: center;
  text-decoration: none;
  color: black;
}

.pokeball {
  width: 30px;        /* adjust size */
  height: 30px;
  margin-right: 0.5rem; /* space between image and text */
}

footer {
  margin-top: 3rem;
  text-align: center;
  color: #666;
  font-size: 0.9rem;
}

</style>
