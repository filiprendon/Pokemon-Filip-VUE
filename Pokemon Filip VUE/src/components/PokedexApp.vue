<template>
    <HelloWorld @show-team="showPokemonTeam" @show-favs="showPokemonFavs">
        <!-- <button v-if="!showTeam" @click="goBack">Volver</button> -->
    </HelloWorld>
    <div class="pokedex-container">

        <div class="content">
            <div v-if="pokemons">
                <PokemonList v-if="!showTeam && !showFavs" :pokemons="pokemons"
                    @addTeam="addTeam" @addFavorite="addFavorite" />
                <PokemonTeam v-if="showTeam && !favs" :team="team"></PokemonTeam>
                <PokemonFavs v-if="showFavs" :favs="favs"></PokemonFavs>
            </div>
        </div>
    </div>
</template>

<script>
import HelloWorld from './HelloWorld.vue'
import PokemonList from './PokemonList.vue'
import PokemonTeam from './PokemonTeam.vue'
import PokemonFavs from './PokemonFavs.vue'
export default {
    components: {
        PokemonList,
        PokemonTeam,
        PokemonFavs,
        HelloWorld
    },
    data() {
        return {
            pokemons: [],
            team: [],
            favs: [],
            showTeam: false,
            showFavs: false,
        }
    },
    mounted() {
    },
    methods: {
        addTeam: function (pokemon) {
            const pokemonInTeam = this.team.some(p => p.id === pokemon.id);
            if (pokemonInTeam) {
                alert('El pokemon ya está en el equipo');
            } else {
                this.team.push(pokemon);
            }
        },
        addFavorite(pokemon) {
            const pokemonIndex = this.favs.findIndex(p => p.id === pokemon.id);
            if (pokemonIndex !== -1) {
                this.favs.splice(pokemonIndex, 1);
                console.log(pokemon, ' Removed from favorites')
                PokemonList.pokemonInFavs = false;
            } else {
                this.favs.push(pokemon);
                console.log(pokemon, ' Added to favorites')
                PokemonList.pokemonInFavs = true;
            }
        },

        showPokemonTeam() {
            this.showTeam = true;
        },
        showPokemonFavs() {
            this.showFavs = true;
        },
        seeList() {
            this.showTeam = false;
        }
    }
}
</script>

<style></style>