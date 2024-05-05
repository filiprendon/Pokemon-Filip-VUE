<template>
    <div class="pokedex-container">
        <nav class="navbar">
            <div class="navbar-brand">
                <h1>Pokédex</h1>
            </div>
        </nav>
        <div class="content">
            <PokemonList :pokemons="pokemons" @addTeam="addTeam" />
            <button @click="showPokemonTeam">Mostrar Equipo Pokémon</button>
            <PokemonTeam v-if="showTeam" :team="team" />
        </div>
    </div>
</template>

<script>
import PokemonList from './PokemonList.vue'
import PokemonTeam from './PokemonTeam.vue'
export default {
    components: {
        PokemonList,
        PokemonTeam
    },
    data() {
        return {
            pokemons: [],
            team: [],
            showTeam: false
        }
    },
    mounted() {
    },
    methods: {
        addTeam: function (pokemon) {
            this.team.push(pokemon);
        },
        showPokemonTeam() {
            this.showTeam = true;
        }
    }
}
</script>

<style></style>