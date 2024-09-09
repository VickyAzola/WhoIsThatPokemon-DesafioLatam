<script>
export default {
  name: 'PokemonCard',
  props: ['pokemonName', 'pokemonImg'],
  data() {
    return {
      userInput: '',
      correct: null,
    }
  },
  computed: {
    applyFilter() {
      return this.correct === false || this.correct === null;
    }
  },
  methods: {
    sendName() {
      if(this.userInput.toLowerCase().trim() == this.pokemonName.toLowerCase()) {
        this.correct = true
        this.$emit('correct-num', this.correct)
      } else {
        this.correct = false
      }
    }
  }
}
</script>

<template>
  <div class="card">
    <img 
      :class="applyFilter ? 'filter' : '' " 
      :src="pokemonImg" 
      alt="Un Pokémon Misterioso" 
      draggable="false"
    />
    <div v-if="correct">
      <p class="name">{{ pokemonName }}</p>
    </div>
    <form v-else @submit.prevent="sendName">
      <input v-model="userInput" required/>
      <button>Descubrir</button>
      <p class="incorrect" v-show="correct == false">Incorrecto, Intenta Nuevamente</p>
    </form>
  </div>
</template>

<style scoped>
.card, form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: .5rem;
}

img {
  user-select: none;
}

.filter {
  filter: blur(5px) grayscale(100%);
}

.name {
  text-align: center;
}

input, button {
  padding: .3rem;
}

.incorrect {
  color: red;
  text-align: center;
  font-size: .9rem;
}
</style>