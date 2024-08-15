<template>
  <div id="app" class="p-4 ">
    <div class="row d-flex justify-content-center gap-3">
      <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRn74k9jrdLZiWw0dCfb06gfj7SzsJbSBR0cQ&s" style="width: 20rem;" alt="Pokemon">
      <h1 class="text-center fs-1 p-2">¿Quién es ese Pokémon?</h1>
      <p class="text-center fs-4 fw-bold pb-2">Pokemones descubiertos: <span class="text-warning">{{ pokemonesDescubiertos }}</span></p>
    </div>

    <div class="row d-flex justify-content-center gap-3">
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
      pokemonesDescubiertos: 0,
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
      this.pokemonesDescubiertos++;
    }
  }
}
</script>

<style>

</style>
