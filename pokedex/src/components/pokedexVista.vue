<template>
  <h2>{{ this.title }} </h2>
  <div v-if="!pokemon">Loading...</div>
  <div v-else>

    <div class="container">

      <div class="row ">
        <div class="col-lg-3 col-md-4 col-sm-5 mb-4" v-for="pokemons in pokemon" :key="pokemons.id" id="listPokemons">

          <div class="card" :id="pokemons.id" :style="getCardStyle(pokemons)">
            <div class="card-body" >
              <img :src="pokemons.sprites.front_default" class="card-img my-3" alt="Pokemon Image">
              <div class="card-img-overlay">
                <div class="d-flex justify-content-between">
                  <button @click="updateEquip(pokemons)" :id="pokemons.id + 'equipF'">EQUIP PKMN</button>
                  <button class="d-none" @click="updateEquip(pokemons)" :id="pokemons.id + 'equipNF'">TEAM UP PKMN</button>
                  <h5 class="card-title">N.° #{{ pokemons.id.toString().padStart(3, '0')}} </h5>
                  <button @click="addFav(pokemons)" :id="pokemons.id + 'fav'" class="noButton"><span class="fa fa-star" ></span></button>
                </div>
                
                


              </div>
              <div>
                <div>
                  <h2> {{ pokemons.name }}</h2>
                  <h4> Types: {{ getTypes(pokemons) }}</h4>
                  <p class="card-text"><small> height {{ pokemons.height }} / weight {{ pokemons.weight }}</small></p>
                </div>
              </div>
            </div>
          </div>

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
      activeFav: false,
      colors: {
    normal: '#CFCFCF',
    fighting: '#FFBF7F',
    flying: '#C0DCF7',
    poison: '#C89EE5',
    ground: '#C8A78C',
    rock: '#D7D4C0',
    bug: '#C8D088',
    ghost: '#B79EB7',
    steel: '#AFD0DB',
    fire: '#F29091',
    water: '#91BFF7',
    grass: '#9DD091',
    electric: '#FCDF7F',
    psychic: '#F69EBC',
    ice: '#9DEBFF',
    dragon: '#A6AFF0',
    dark: '#008080',
    fairy: '#F6B7F7',
    unknown: '#F9F9F9',
    shadow: '#A69E9D',

      },
      equipo: [],
      equipCount: 0,
    };
  },

  methods: {
    fetchData() {
      const maxPokemon = 9
      fetch('https://pokeapi.co/api/v2/pokemon/?limit=' + maxPokemon)
        .then(response => response.json())
        .then(data => {

          Promise.all(data.results.map(pokemon => fetch(pokemon.url).then(response => response.json())))
            .then(pokemonDetails => {
              this.pokemon = pokemonDetails;
            })
            .catch(err => console.log("Error fetching Pokemon details: " + err.message));
        })
        .catch(err => console.log("Error fetching API: " + err.message));
    }, 
    getTypes(pokemon) {
      return pokemon.types.map(type => type.type.name).join(" / ");
    },
    addFav(pokemon){
      const fav = document.getElementById(pokemon.id+'fav')
       
      if(this.activeFav){
        this.activeFav = false
        fav.classList.remove('fav')
        fav.classList.add('noFav')

      } else{

        this.activeFav = true
        fav.classList.remove('noFav')
        fav.classList.add('fav')

      }

    },
    getCardStyle: function(pokemon) {
      const pokemonType = pokemon.types.map(type => type.type.name)
        if (pokemonType.length === 1) {
          return { backgroundColor: this.colors[pokemonType[0]] };
        } else if (pokemonType.length === 2) {  
        return {
            background: `linear-gradient(to right, ${this.colors[pokemonType[0]]}, ${this.colors[pokemonType[1]]})`
          };
        } 
      },
      updateEquip(pokemon){

        const pokemonId = pokemon.id
        const exist = this.equipo.find(p => p.id === pokemonId)
        if (exist) {


          if (this.equipCount === 0) {
            alert('El equipo esta vacio')

          }else{
            this.equipCount -= 1

            const buttonF = document.getElementById(pokemon.id+'equipF');
            buttonF.classList.remove('d-none')
            const buttonNF = document.getElementById(pokemon.id+'equipNF');
            buttonNF.classList.add('d-none')

            const index = this.equipo.findIndex(p => p.id === pokemon.id);
            if (index !== -1) {
                this.equipo.splice(index, 1); 
            }   
          }

          
        } else {
          this.equipCount += 1

          if (this.equipCount < 7 ) {
            this.equipo.push(pokemon)
            const buttonF = document.getElementById(pokemon.id+'equipF');
            buttonF.classList.add('d-none')
            const buttonNF = document.getElementById(pokemon.id+'equipNF');
            buttonNF.classList.remove('d-none')
            console.log(this.equipo)
          } else {
            alert('El equipo esta completo')
          }
        }
        

      }
  },
  mounted() {
    this.fetchData();
  },
};
</script>
<style>
.card-img {
  width: 70%;
}
.noButton{
  border: none;
  background-color: white;
}
.fav{
  color: gold;
}
.noFav{
  color: black;
}
</style>