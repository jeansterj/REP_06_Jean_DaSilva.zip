<template>
<h2>{{ this.title }} </h2>
  <div v-if="!pokemon">Loading...</div>
  <div v-else>

      <div class="container" v-for="pokemons in pokemon" :key="pokemons.id" id="listPokemons">


        <div class="card" :id="pokemon.id">
          <div class="bodyCard">
          <h2> {{ pokemons.name }}</h2>
          <img :src="pokemons.sprites.front_default" alt="Pokemon Image">
          <h4 > Tipos: {{ getTypes(pokemons) }}</h4>
          
        </div>
          
        </div>


      </div>
  </div>



</template>
<script>
export default {
  data() {
    return {
      title: "Pokedex",
      pokemon: null,
    };
  },
  methods: {
    fetchData() {
      const maxPokemon = 50
      fetch('https://pokeapi.co/api/v2/pokemon-form/?limit=' + maxPokemon)
        .then(response => response.json())
        .then(data => {

          Promise.all(data.results.map(pokemon => fetch(pokemon.url).then(response => response.json())))
            .then(pokemonDetails => {
              this.pokemon = pokemonDetails;
            })
            .catch(err => console.log("Error fetching Pokemon details: " + err.message));
        })
        .catch(err => console.log("Error fetching API: " + err.message));
    },getTypes(pokemon) {
  return pokemon.types.map(type => type.type.name).join(" / ");
},
  },
  mounted() {
    this.fetchData();
  },
};
</script>