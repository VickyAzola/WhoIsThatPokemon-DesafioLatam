<script setup>
import PokemonApp from './components/PokemonApp.vue'
import Logo from '/pokemon-logo.png'
</script>

<template>
  <img :src="Logo" />
  <h1>¿Quién es ese Pokémon?</h1>
  <PokemonApp />
</template>

<style scoped>
img {
  display: block;
  margin: 1rem auto;
  width: 250px;
}

h1 {
  text-align: center;
}
</style>
