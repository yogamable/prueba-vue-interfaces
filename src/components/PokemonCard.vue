<template>
    <div class="card d-flex" style="width: 18rem;">
      <div >
        <img :src="pokemon.imagen" class="card-img-top" alt="pokemon.name" :style="{height: altura, filter: filtroImg}" >
      </div>
      <div class="card-body">
        <h5 class="card-title text-center">¡Descubre su nombre!</h5>
        <label class="d-flex justify-content-center" id="nombrePokemon" >{{ pokemon.nombre }}</label>
        <div class="d-flex justify-content-center mt-2">
          <input type="text" v-model="inputPokemon" />
        </div>
        <div class="d-flex justify-content-center">
          <button type="submit" @click="descubrirPokemon()" class="btn btn-primary mt-2">Descubrir</button>
        </div>        
      </div>
    </div>

      
</template>

<script>

export default {
  name: 'PokemonCard',
  data(){
    return{
      altura: '13rem',
      inputPokemon: '',
      filtroImg: "blur(10px)",
    }
  },

  props: {
    pokemon: Object,
  },  

  methods: {
    
    descubrirPokemon(){
     if(this.inputPokemon == this.pokemon.nombre){
        alert("¡Felicitaciones, adivinaste!");
        this.filtroImg = 'none';
        this.$emit('descubrirPokemon');
     }else {
      alert("¡Te equivocaste!");
     }
    },

  },
}
</script>

<style scoped>

.filtroImg{
  filter: blur(10px);
}

</style>
