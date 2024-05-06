<template>
  <h2>{{ this.title }} </h2>
  <div v-if="!pokemon">Loading...</div>
  <div v-else>

    <div class="container">
      <div class="d-flex justify-content-between my-4 gap-2">

        <button class="btn btn-outline-primary" type="button" data-bs-toggle="modal" data-bs-target="#exampleModal">Show
          Team</button>
        <button class="btn btn-outline-warning" type="button" data-bs-toggle="modal"
          data-bs-target="#favoriteModal">Show Favorite</button>
        <button class="btn btn-outline-danger" type="button" data-bs-toggle="button" @click="showFilter()">show by
          range</button>
      </div>

      <div v-if="!types">Loading...</div>
      <div v-else>
        <div class="d-flex justify-content-between my-4">
          <button class="btn btn-light" @click="ShowAll()">Show All Pokemons</button>
          <button v-for="type in types" :key="type.name" :style="getCardStyle(type)" class="btn"
            @click="updateSelectedTypes(type)">{{ type.name }}</button>

        </div>
      </div>
      <div v-if="filter">
        <slider @filterPokemonRange="filterPokemonRange"></slider>
      </div>

      <div class="row ">
        <div class="col-lg-3 col-md-4 col-sm-5 mb-4" v-for="pokemons in filtrePokemons" :key="pokemons.id"
          id="listPokemons">

          <div class="card" :id="pokemons.id" :style="getCardStyle(pokemons)">
            <div class="card-body">
              <img :src="pokemons.sprites.front_default" class="card-img my-3" alt="Pokemon Image">
              <div class="card-img-overlay">
                <div class="d-flex justify-content-between">
                  <button @click="updateEquip(pokemons)" :id="pokemons.id + 'equipF'">EQUIP PKMN</button>
                  <button class="d-none" @click="updateEquip(pokemons)" :id="pokemons.id + 'equipNF'">TEAM UP
                    PKMN</button>
                  <h5 class="card-title">N.° #{{ pokemons.id.toString().padStart(3, '0') }} </h5>
                  <button @click="addFav(pokemons)" :id="pokemons.id + 'fav'" class="noButton"><span
                      class="fa fa-star"></span></button>
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


  <!-- Modal Equipo -->
  <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered modal-xl">
      <div class="modal-content">
        <div class="modal-header">
          <h1 class="modal-title fs-5 text-white" id="exampleModalLabel">Equip PKMN List</h1>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="container">
          <div class="row">

            <div v-if="equipo.length < 6">
              <h5 class="text-white my-5"> No has completado el equipo, deben ser 6 </h5>
            </div>

            <div v-else class="modal-body col-lg-4 col-md-4 col-sm-5 mb-4" v-for="equipos in equipo" :key="equipos.id"
              id="equipList">
              <div class="card" :id="equipos.id" :style="getCardStyle(equipos)">
                <div class="card-body">
                  <img :src="equipos.sprites.front_default" class="card-img my-3" alt="Pokemon Image">
                  <div class="card-img-overlay">
                    <div class="d-flex justify-content-between">
                      <button @click="updateEquip(equipos)" :id="equipos.id + 'equipF'">EQUIP PKMN</button>
                      <button class="d-none" @click="updateEquip(equipos)" :id="equipos.id + 'equipNF'">TEAM UP
                        PKMN</button>
                      <h5 class="card-title">N.° #{{ equipos.id.toString().padStart(3, '0') }} </h5>
                      <button @click="addFav(equipos)" :id="equipos.id + 'fav'" class="noButton"><span
                          class="fa fa-star"></span></button>
                    </div>

                  </div>
                  <div>
                    <div>
                      <h2> {{ equipos.name }}</h2>
                      <h4> Types: {{ getTypes(equipos) }}</h4>
                      <p class="card-text"><small> height {{ equipos.height }} / weight {{ equipos.weight }}</small></p>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- Modal Favorito -->
  <div class="modal fade" id="favoriteModal" tabindex="-1" aria-labelledby="favoriteModal" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered modal-xl modal-dialog-scrollable">
      <div class="modal-content">
        <div class="modal-header">
          <h1 class="modal-title fs-5 text-white" id="exampleModalLabel">Favorite List</h1>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>


        <div class="container">
          <div class="row overflowFav">
            <div v-if="favorite.length === 0">
              <h5 class="text-white my-5"> No hay favoritos </h5>
            </div>
            <div v-else class="modal-body col-lg-4 col-md-4 col-sm-5 mb-4 " v-for="favorites in favorite"
              :key="favorites.id" id="equipList">
              <div class="card" :id="favorites.id" :style="getCardStyle(favorites)">
                <div class="card-body">
                  <img :src="favorites.sprites.front_default" class="card-img my-3" alt="Pokemon Image">
                  <div class="card-img-overlay">
                    <div class="d-flex justify-content-between">
                      <button @click="updateEquip(favorites)" :id="favorites.id + 'equipF'">EQUIP PKMN</button>
                      <button class="d-none" @click="updateEquip(favorites)" :id="favorites.id + 'equipNF'">TEAM UP
                        PKMN</button>
                      <h5 class="card-title">N.° #{{ favorites.id.toString().padStart(3, '0') }} </h5>
                      <button @click="addFav(favorites)" :id="favorites.id + 'fav'" class="noButton"><span
                          class="fa fa-star"></span></button>
                    </div>

                  </div>
                  <div>
                    <div>
                      <h2> {{ favorites.name }}</h2>
                      <h4> Types: {{ getTypes(favorites) }}</h4>
                      <p class="card-text"><small> height {{ favorites.height }} / weight {{ favorites.weight }}</small>
                      </p>
                    </div>
                  </div>
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
import slider from "./SlidePokemon.vue";

export default {
  components: {
    slider
  },
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
      equipCount: 1,
      favorite: [],
      types: null,
      selectType: null,
      filter: false,
      filterBusq: false,
      minRange: 0,
      maxRange: 0
    }
  },

  methods: {
    fetchData() {
      const maxPokemon = 151
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
    fetchTypes() {
      fetch('https://pokeapi.co/api/v2/type')
        .then(response => response.json())
        .then(data => {
          this.types = data.results;
        })
        .catch(error => {
          console.error('Error fetching Pokemon types:', error);
        });
    },
    showFilter() {
      this.filter = !this.filter;
      this.filterBusq = !this.filterBusq;

    },
    filterPokemonRange(minRange, maxRange) {
      this.minRange = minRange
      this.maxRange = maxRange
    },

    getTypes(pokemon) {
      return pokemon.types.map(type => type.type.name).join(" / ");
    },
    addFav(pokemon) {

      const fav = document.getElementById(pokemon.id + 'fav')
      const pokemonId = pokemon.id

      const exist = this.favorite.find(p => p.id === pokemonId)

      if (exist) {

        fav.classList.remove('fav')
        fav.classList.add('noFav')

        const index = this.favorite.findIndex(p => p.id === pokemon.id);

        if (index !== -1) {
          this.favorite.splice(index, 1);
          console.log(this.favorite)
        }

      } else {

        fav.classList.remove('noFav')
        fav.classList.add('fav')
        this.favorite.push(pokemon)
        console.log(this.favorite)
      }

    },
    getCardStyle: function (pokemonOrType) {
      if (pokemonOrType.types) {
        const pokemonType = pokemonOrType.types.map(type => type.type.name);
        if (pokemonType.length === 1) {
          return { backgroundColor: this.colors[pokemonType[0]] };
        } else if (pokemonType.length === 2) {
          return {
            background: `linear-gradient(to right, ${this.colors[pokemonType[0]]}, ${this.colors[pokemonType[1]]})`
          };
        }
      }
      else {
        const type = pokemonOrType;
        return { backgroundColor: this.colors[type.name] };
      }
    },
    updateEquip(pokemon) {
      const pokemonId = pokemon.id
      const exist = this.equipo.find(p => p.id === pokemonId)
      if (exist) {

        const buttonF = document.getElementById(pokemon.id + 'equipF');
        buttonF.classList.remove('d-none')
        const buttonNF = document.getElementById(pokemon.id + 'equipNF');
        buttonNF.classList.add('d-none')

        const index = this.equipo.findIndex(p => p.id === pokemon.id);
        if (index !== -1) {
          this.equipo.splice(index, 1);
          this.equipCount -= 1
        }
      } else {
        if (this.equipCount < 7) {
          this.equipCount += 1
          this.equipo.push(pokemon)
          const buttonF = document.getElementById(pokemon.id + 'equipF');
          buttonF.classList.add('d-none')
          const buttonNF = document.getElementById(pokemon.id + 'equipNF');
          buttonNF.classList.remove('d-none')

        } else {
          alert('El equipo esta completo')
        }
      }
    },
    updateSelectedTypes(selectedTypes) {
      this.selectType = selectedTypes;
    },
    ShowAll() {
      this.selectType = null
    }
  },
  computed: {
    filtrePokemons() {
      if (this.filterBusq) {
        return this.pokemon.filter(pokemon => pokemon.id >= this.minRange && pokemon.id <= this.maxRange);
      } else if (!this.selectType) {
        return this.pokemon;
      } else {
        return this.pokemon.filter(pokemon => {
          return pokemon.types.some(type => type.type.name === this.selectType.name);
        });
      } 

    }
  },
  mounted() {
    this.fetchData();
    this.fetchTypes();
  },
};
</script>
<style>
.card-img {
  width: 70%;
}

.noButton {
  border: none;
  background-color: white;
}

.fav {
  color: gold;
}

.noFav {
  color: black;
}

body {
  background-color: #25292B
}

.modal-content {
  background-color: #25292B;
}

button.btn-close {
  color: white !important;
  background-color: white;
}

.overflowFav {

  overflow: auto;
  height: 420px;
}
</style>