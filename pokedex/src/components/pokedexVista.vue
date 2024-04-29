<template>
  <h2>{{ this.title }} </h2>




</template>
<script>
export default {
  data() {
    return {
      title: "Pokedex",
      pokemons: null,
    };
  },
  methods: {
    fetchData() {
      const maxPokemon = 151
      fetch('https://pokeapi.co/api/v2/pokemon-form/?limit=' + maxPokemon)
        .then(response => response.json())
        .then(data => {
            data.results.forEach(pokemon => {
                fetch(pokemon.url)
                .then(response => response.json())
                
            });

        })
        .catch(err => console.log("Error fetching API: " + err.message));
    }
  },
  mounted() {
    this.fetchData();
  },
};
</script>