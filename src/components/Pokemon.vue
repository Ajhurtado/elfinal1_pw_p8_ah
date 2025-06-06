<template>
    <div v-if="!ganador && !perdedor" class="contenedor1">
        <div class="pokemon-container1.1">
            <div>
                <h1>Puntaje: {{ puntaje }}</h1>
                <h1>Intentos: {{ intentos }}</h1>
            </div> 
            <div >
                <img v-if="mostrar" class="card1" :src="imagenFuente" alt="">
                <img v-if="!mostrar" class="oculto" :src="imagenFuente" alt="">
                <p v-if="mostrar">{{ textoOculta1 }}</p>
                <p v-if="!mostrar">XXXXXXXXXXX</p>
            </div>
            <div>  
                <img v-if="mostrar" class="card2" :src="imagenFuente2" alt="">
                <img v-if="!mostrar" class="oculto" :src="imagenFuente" alt="">
                <p v-if="mostrar">{{ textoOculta2 }}</p>
                <p v-if="!mostrar">XXXXXXXXXXX</p>
            </div>
            <div>
                <img v-if="mostrar" class="card3" :src="imagenFuente3" alt="">
                <img v-if="!mostrar" class="oculto" :src="imagenFuente" alt="">
                <p v-if="mostrar">{{ textoOculta3 }}</p>
                <p v-if="!mostrar">XXXXXXXXXXX</p>
            </div>
        </div>            
            <div class="botones">
            <button v-on:click="pokemonIguales">Jugar</button>
        </div>
        <div class="contenedor1.2">
            <div class="MostrarGanador" v-if="ganador">
                <p>Puntaje: #{{ puntaje }}</p>
                <p>Felicitaciones has ganado un premio de $10.000,00</p>
            </div>
            <div class="MostrarPerdedor" v-if="perdedor">
                <h1 class="intentos5">Has utilizado tus 5 intentos</h1>
                <p>El juego a terminado, intentalo de nuevo</p>
            </div>
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
        // Asigna los nombres a los textos ocultos
        this.textoOculta1 = this.pokemons[0]?.nombre || "???";
        this.textoOculta2 = this.pokemons[1]?.nombre || "???";
        this.textoOculta3 = this.pokemons[2]?.nombre || "???";
    },
    pokemonIguales() {
        this.mostrar = true; // Muestra las imágenes
        this.intentos++;
        if (this.pokemonId === this.pokemonId2 && this.pokemonId2 === this.pokemonId3) {
          this.puntaje += 5;
        } else if (
          this.pokemonId === this.pokemonId2 ||
          this.pokemonId === this.pokemonId3 ||
          this.pokemonId2 === this.pokemonId3
        ){
          this.puntaje += 3;
        } else {
          this.puntaje += 0;
        }
        if(this.puntaje >= 15) {
        this.ganador = true; // Marca al jugador como ganador
        return; // Sale del método si se ha ganado
        }
        if(this.intentos === 5){
        this.perdedor = true; // Marca al jugador como perdedor
        return; // Sale del método si se ha perdido
        }
        this.obtenerPokemones(); // Nuevas imágenes al jugar
    },
    resetGame() {
        this.puntaje = 0;
        this.intentos = 0;
        this.mostrarTexto = false;
        this.mostrar = false; // Oculta las imágenes
        this.ganador = false;
        this.perdedor = false;
        this.textoOculta1 = "XXXXXXX";
        this.textoOculta2 = "XXXXXXX";
        this.textoOculta3 = "XXXXXXX";
        this.obtenerPokemones();
    }
    },
}
</script>


<style>

.oculto{
    filter: brightness(0);
}
.pokemon-container {
  display: flex;
  flex-direction: row;
  align-items: center;
  margin-bottom: 20px;
}

.MostrarGanador, .MostrarPerdedor {
  display: flex;
  justify-content: center;
  text-align: center;
  margin-top: 20px;
  border: solid 2px black;
  padding: 10px;
}

.intentos5 {
  color: blue;
  font-size: 40px;
}
</style>    
