<template>
  <div id="app" class="d-block m-5">
    <div>
      <h1>Descubre tus Pokemon</h1>
    </div>

    <div class="row d-flex gap-3">
      <PokemonCard v-for="(pokemon, index) in pokemones"
      :key="index"
      :pokemon="pokemon"
      @descubrirPokemon="pokemonDescubierto"
      />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import PokemonCard from './components/PokemonCard.vue';

export default {
  name: 'App',
  data(){
    return{
      pokemones: [],
    }
  },
  
  components: {
    PokemonCard
  },

  async mounted (){
    await this.obtenerPokemones();
  },

  methods: {
    async obtenerPokemones(){
      const url = "https://pokeapi.co/api/v2/pokemon?offset=20&limit=20";
      const responseApi = await axios.get(url);
      const primerLlamado = responseApi.data.results;

      //hay que llamar a cada uno de los 20 pokemones

      const pokemones = await Promise.all(
      primerLlamado.map(async (pokemon) => {
      const { data } = await axios.get(pokemon.url);
      return {
        nombre: data.name,
        imagen: data.sprites.front_default,
        inputPokemon: data.inputPokemon,
        
      };
      })
    );
      this.pokemones = pokemones;
    },

    pokemonDescubierto(){
      
    }
  }
}
</script>

<style>

</style>
