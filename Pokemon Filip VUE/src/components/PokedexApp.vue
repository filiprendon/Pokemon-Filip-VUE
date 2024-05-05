<template>
    <div class="pokedex-container">
        <nav class="navbar" style="margin-top: 50px;" @showTeam="showPokemonTeam">
            <div class="navbar-brand">
                <h1>Pokédex</h1>
            </div>
        </nav>
        <div class="content">
            <PokemonList :pokemons="pokemons" @addTeam="addTeam" />
            <PokemonTeam v-if="showTeam" :team="team"></PokemonTeam>
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
            const pokemonInTeam = this.team.some(p => p.id === pokemon.id);
            if (pokemonInTeam) {
                alert('El pokemon ya está en el equipo');
            } else {
                this.team.push(pokemon);
            }
        }
        ,
        showPokemonTeam() {
            this.showTeam = true;
        }
    }
}
</script>

<style>
</style>