<template>
    <div style="text-align: center;">
        <h1 v-if="!pokemon">Espere...</h1>
        <div v-else="pokemon">
            <h1>¿QUIEN ES ESTE POKEMON?</h1>
            <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon" />
            <PokemonOptions :pokemons="pokemonArr" @selectionPokemon="checkAnswer" />
            <template v-if="showAnswer">
                <h2 class="fade-in">{{ message }}</h2>
                <button @click="newGame">Nuevo Juego</button>
            </template>
        </div>
    </div>
</template>

<script>
import PokemonOptions from '@/components/PokemonOptions.vue'
import PokemonPicture from '@/components/PokemonPicture.vue'
import getPokemonOptions from '@/helpers/getPokemonOptions'
export default {
    components: { PokemonPicture, PokemonOptions },
    data() {
        return {
            pokemonArr: [],
            pokemon:null,
            showPokemon: false,
            showAnswer: false,
            message: ''
        }
    },
    methods: {
        async mixPokemonArray() {
            this.pokemonArr = await getPokemonOptions()
            const rndInt = Math.floor( Math.random() * 4 )
            this.pokemon = this.pokemonArr[ rndInt ]
        },
        checkAnswer(selectedId) {
            this.showPokemon = true
            this.showAnswer = true
            if(selectedId === this.pokemon.id) {
                this.message = `Correcto, ${ this.pokemon.name}`
            } else {
                this.message = `Incorrecto, era ${ this.pokemon.name}`
            }
        },
        newGame() {
            this.showPokemon = false
            this.showAnswer = false
            this.pokemonArr = []
            this.pokemon = null
            this.mixPokemonArray()
        }
    },
    mounted() {
        this.mixPokemonArray()
    }
}
</script>