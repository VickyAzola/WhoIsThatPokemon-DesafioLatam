<script>
import axios from 'axios'
import PokemonCard from './PokemonCard.vue'

export default {
  name: 'PokemonApp',
  components: { PokemonCard },
  data() {
    return {
      pokemonList: [],
      counter: 0
    }
  },
  async created() {
    try {
      const url = 'https://pokeapi.co/api/v2/pokemon?limit=20'
      const { data } = await axios.get(url) 
      const promises = data.results.map(async (pokemon) => { 
        const pokemonDetail = await axios.get(pokemon.url)
        return {
          name: pokemonDetail.data.name,
          img: pokemonDetail.data.sprites.front_default 
        }
      })
      
      this.pokemonList = await Promise.all(promises) 
      
    } catch (error) {
      console.error('No pudimos atrapar ningún pokemón 😞:' + error)
    }
  },
  methods: {
    counterCorrects(correct) {
      if(correct) {
        this.counter++
      }
    }
  }
}
</script>

<template>
  <div class="counter">
    <p>Pokémones Descubiertos: <span>{{ counter }}</span></p>
  </div>
  <div v-if="pokemonList.length" class="cards-container">
    <PokemonCard 
      v-for="(pokemon, index) in pokemonList" 
      :key="index"
      :pokemonName="pokemon.name"
      :pokemonImg="pokemon.img"
      @correct-num="counterCorrects"
    />
  </div>
</template>

<style scoped>
.counter {
  margin: 1rem auto 2rem;
  text-align: center;
}

.counter span {
  color: rgb(255, 224, 46);
  font-weight: bold;
  font-size: 1.3rem;
}

.cards-container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  gap: 3rem;
}

@media screen and (width < 950px) {
  .cards-container {
    grid-template-columns: 1fr 1fr 1fr;
  }
}

@media screen and (width < 700px) {
  .cards-container {
    grid-template-columns: 1fr 1fr;
  }
}
</style>