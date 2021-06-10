<template>
  <div id="app">
    <h3 class="is-size-3">Pokedex</h3>

    <input
      v-model="busca"
      class="input is-rounded"
      type="text"
      placeholder="Buscar Pokemon pelo nome"
      @keyup="buscar"
    />

    <hr />
    <div class="columns is-multiline">
      <div
        v-for="(pokemon, index) in pokemonsFiltrados"
        :key="pokemon.url"
        class="column is-one-quarter"
      >
        <Pokemon :name="pokemon.name" :url="pokemon.url" :num="index + 1" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon.vue";

export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      pokemonsFiltrados: [],
      busca: "",
    };
  },
  created: function () {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        this.pokemons = res.data.results;
        this.pokemonsFiltrados = res.data.results;
      });
  },
  components: {
    Pokemon,
  },
  methods: {
    buscar: function () {
      this.pokemonsFiltrados = this.pokemons;
      if (this.busca == "" || this.busca == " ") {
        this.pokemonsFiltrados = this.pokemons;
      } else {
        this.pokemonsFiltrados = this.pokemons.filter((pokemon) =>
          pokemon.name.match(this.busca.toLocaleLowerCase())
        );
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
