<template>
    <HelloWorld @show-team="showPokemonTeam" @show-favs="showPokemonFavs">
        <!-- <button v-if="!showTeam" @click="goBack">Volver</button> -->
    </HelloWorld>
    <div class="pokedex-container">

        <div class="content">
            <PokemonList v-if="!showTeam" :pokemons="pokemons" @addTeam="addTeam" />
            <PokemonTeam v-if="showTeam" :team="team"></PokemonTeam>
            <PokemonFavs v-if="showFavs" :favs="favs"></PokemonFavs>
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
        addFavorite() {
            const pokemonInFavs = this.favs.some(p => p.id === pokemon.id);
            if (pokemonInFavs) {
                alert('El pokemon ya está en el equipo');
            } else {
                this.favs.push(pokemon);
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