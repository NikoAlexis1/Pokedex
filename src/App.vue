<script>
import axios from 'axios';
import PokemonCard from './components/PokemonCard.vue';
export default {

  data() {
    return {
      pokemons: [],
    }
  },
  components: {
    PokemonCard
  },
  methods: {
    async getPokemons() {
      const { data } = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=10');
      this.pokemons = await Promise.all(data.results.map(async (poke) => {
        const { data: pokemonData } = await axios.get(poke.url);
        return {
          id: pokemonData.id,
          name: pokemonData.name,
          image: pokemonData.sprites.front_default,
        };
      }));
      console.log(this.pokemons);
    }
  },
  mounted() {
    this.getPokemons();
  }
}

</script>

<template>
  <PokemonCard />
  <p v-for="pokemon in pokemons">{{ pokemon.name }}</p>
</template>

<style scoped></style>
