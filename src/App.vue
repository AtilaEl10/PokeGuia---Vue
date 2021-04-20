<template>
  <div id="app">
    <img src="https://raw.githubusercontent.com/PokeAPI/media/master/logo/pokeapi_256.png" alt="Logo PokeApi">
    <div class="col-12 mt-5">
      <h2 class="fw-bold">PokeGuia</h2>
    </div>
    <div class="container">
      <div class="row">
        <div class="col-8 mx-auto mt-3">
          <label for="form-label mt-5">Nombre del Pokémon:</label>
          <input v-model="nombre_pokemon" type="text" class="form-control mt-3 text-center" placeholder="">
        </div>
        <div class="col-6 mx-auto mt-4">
          <button @click="getPoke" type="button" class="btn btn-danger">Buscar</button>
        </div>

        <div class="container my-3">
        <!-- Imagen Pokemon-->
          <div class="row">
            <div class="col-10 mx-auto">
              <img class="mx-2 sprite" v-if="pokeData" :src="pokeData && pokeData.sprites && pokeData.sprites.front_default" alt="Pokemon">
              <img class="mx-2 sprite" v-if="pokeData" :src="pokeData && pokeData.sprites && pokeData.sprites.front_shiny" alt="Pokemon Shinny">
            </div>
          </div>
          <!-- Habilidad-->
          <div class="row mt-3">
            <div class="col-12 mt-3">
              <h5 v-if="pokeData" class="fw-bold mb-3">HABILIDADES</h5>
              <div class="container">
                <div class="row">
                  <div class="col-12 text-capitalize" v-for="(ability, i) in pokeData.abilities" :key="i">{{ability.ability.name}}</div>
                </div>
              </div>
            </div>
          </div>
          <!-- Movimientos-->
          <div class="row">
            <div class="col-12 mt-3">
              <h5 v-if="pokeData" class="fw-bold mb-4">MOVIMIENTOS</h5>
              <div class="container">
                <div class="row">
                  <div class="col-4 text-capitalize" v-for="(move, i) in pokeData.moves" :key="i">{{move.move.name}}</div>
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
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      nombre_pokemon: "pikachu",
      pokeData: "",
    }
  },

  methods: {
    async getPoke() {
      const url = "https://pokeapi.co/api/v2/pokemon/"
      try {
        const request = await axios(url + this.nombre_pokemon.toLowerCase().replaceAll(' ', '-'));
        if(!request) return
        this.pokeData = request.data;
        // console.log(request.data);
        this.nombre_pokemon = ""
      } catch (error) {
        console.log(`Error en la llamada a la API: ${error}`);
      }
    }
  },
  created() {
    this.getPoke()
  }

};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 40px;
}
.sprite {
  width: 170px;
}
</style>
