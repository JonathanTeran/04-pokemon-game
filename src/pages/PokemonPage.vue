<template>
  <h1 v-if="!pokemon">Espere por favor...</h1>
  <div v-else>
    <h1>Quién es este Pokemón?</h1>
    <PokemonPicture 
    :pokemonId="pokemon.id" 
    :showPokemon="showPokemon"/>

    <PokemonOptions 
    :pokemons="pokemonArr"
    @selection-pokemon="checkAnswer"/>

    <template v-if="showAnswer" >
        <h2 class="fade-in">{{ messaje }}</h2>
        <button @click="newGame">
          Nuevo Juego
        </button>
    </template>
  </div>
</template>
<script>
import PokemonOptions from '@/components/PokemonOptions'
import PokemonPicture from '@/components/PokemonPicture'
import getPokemonOptions from '@/helper/getPokemonOptions'
export default {
    components: {PokemonOptions,PokemonPicture},
    data(){
      return {
        pokemonArr: [],
        pokemon:    null,
        showPokemon:false,
        showAnswer: false,
        messaje:    ''
      }
    },
    methods: {
      async mixPokemonArray(){
        this.pokemonArr = await getPokemonOptions()
        const rndInt    = Math.floor(Math.random() * 4)
        this.pokemon    = this.pokemonArr[rndInt]
      },
      checkAnswer(selectedId){
        this.showPokemon  = true
        this.showAnswer   = true
        if(selectedId === this.pokemon.id){
          this.messaje = `Correcto, ${ this.pokemon.name} :)`
        }else{
          this.messaje = `Oops, era ${ this.pokemon.name} :(`
        }
      },
      newGame (){
        this.pokemonArr   = []
        this.pokemon      = null
        this.showPokemon  = false
        this.showAnswer   = false
        this.messaje      = ''
        this.mixPokemonArray()
      }
    },
    mounted(){
      this.mixPokemonArray()
    }
}
</script>