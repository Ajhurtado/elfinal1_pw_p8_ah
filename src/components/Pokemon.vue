<template>
    <div class="contenedoruno">
        <div class="escores">
                <h1>Puntaje: {{ puntaje }}</h1>
                <h1>Intentos: {{ intentos }}</h1>
        </div> 
        <div class="pokemon-containerunouno">
            <div  >
                <img v-if="mostrar"  :src="imagenFuente" alt="">
                <img v-if="!mostrar" class="oculto" :src="imagenFuente" alt="">
                <p v-if="mostrar">{{ textoOculta1 }}</p>
                <p v-if="!mostrar">XXXXXXXXXXX</p>
            </div>
            <div >  
                <img v-if="mostrar"  :src="imagenFuente2" alt="">
                <img v-if="!mostrar" class="oculto" :src="imagenFuente" alt="">
                <p v-if="mostrar">{{ textoOculta2 }}</p>
                <p v-if="!mostrar">XXXXXXXXXXX</p>
            </div>
            <div >
                <img v-if="mostrar"  :src="imagenFuente3" alt="">
                <img v-if="!mostrar" class="oculto" :src="imagenFuente" alt="">
                <p v-if="mostrar">{{ textoOculta3 }}</p>
                <p v-if="!mostrar">XXXXXXXXXXX</p>
            </div>
        </div>     
        <div class="contenedorunodos">
            <div class="MostrarGanador" v-if="ganador">
                <p class="puntaje">Puntaje: #{{ puntaje }}</p>
                <p class="puntajeuno">Felicitaciones has ganado un premio de $10.000,00</p>
            </div>
            <div class="MostrarPerdedor" v-if="perdedor">
                <h1 class="intentos">Has utilizado tus 5 intentos</h1>
                <p class="intentosuno">El juego a terminado, inténtelo otra vez</p>
            </div>
        </div>         
         <div class="botones">
                <button v-on:click="pokemonIguales">Jugar</button>
        </div>
  </div>

</template>

<script>
import {obtenerOpcionesFachada} from '@/client/PokemonClient.js'

export default {
  data() {
    return {
        pokemons: [],
        pokemonId: null,
        pokemonId2: null,
        pokemonId3: null,
        mostrar: false, 
        puntaje: 0, 
        intentos: 0, 
        ganador: false,
        perdedor: false, 
        textoOculta1: "XXXXXXX", 
        textoOculta2: "XXXXXXX", 
        textoOculta3: "XXXXXXX", 
        mostrarTexto: false 

      }
  },
  computed: {
         imagenFuente() {
      return `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${this.pokemonId}.svg`;
    },
        imagenFuente2() {
      return  `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${this.pokemonId2}.svg`;
    },
        imagenFuente3() {
      return `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${this.pokemonId3}.svg`;
     },
    },

   methods: {
    async obtenerPokemones() {
        this.pokemons = await obtenerOpcionesFachada(3);
        this.pokemonId = this.pokemons[0].id;
        this.pokemonId2 = this.pokemons[1].id;
        this.pokemonId3 = this.pokemons[2].id;
        this.textoOculta1 = this.pokemons[0]?.nombre || "???";
        this.textoOculta2 = this.pokemons[1]?.nombre || "???";
        this.textoOculta3 = this.pokemons[2]?.nombre || "???";
    },
    pokemonIguales() {
        this.mostrar = true; 
        this.intentos++;
        if (this.pokemonId === this.pokemonId2 && this.pokemonId2 === this.pokemonId3) {
          this.puntaje += 5;
            } else if (
                this.pokemonId === this.pokemonId2 ||
                this.pokemonId === this.pokemonId3 ||
                this.pokemonId2 === this.pokemonId3
            ){
                this.puntaje += 2;
            } else {
                this.puntaje += 0;
            }
            if(this.puntaje >= 10) {
                this.ganador = true; 
                return; 
            }
            if(this.intentos === 5){
                this.perdedor = true; 
                return;
        }
        this.obtenerPokemones(); 
    },
   },
}
</script>


<style>

img{
    height: 200px;
}

.scores{
    display: relative;
    flex-direction: column;
    align-items: center;

}

.oculto{
    filter: brightness(0);
}
.pokemon-containerunouno {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  margin-bottom: 20px;

}
.contenedorunodos{
    display: flex;
    justify-content: center;
}

.MostrarGanador, .MostrarPerdedor {
  display: relative;
  justify-content: center;
  text-align: center;
  margin-top: 20px;
  border: solid 2px black;
  padding: 10px;
}

.intentos, .intentosuno {
  color: red;
  font-size: 40px;
}

.puntaje, .puntajeuno{
    color: blue;
}

.botones {
    display: relative;
}

button {
    padding: 15px;
}
</style>    

