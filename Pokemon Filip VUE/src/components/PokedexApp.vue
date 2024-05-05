<template>
    <HelloWorld @show-team="showPokemonTeam" @go-back="seeList"/>
    <div class="pokedex-container">

        <div class="content">
            <PokemonList v-if="!showTeam" :pokemons="pokemons" @addTeam="addTeam" />
            <PokemonTeam v-if="showTeam" :team="team" @seeList="seeList"></PokemonTeam>
        </div>
    </div>
</template>

<script>
import HelloWorld from './HelloWorld.vue';
import PokemonList from './PokemonList.vue'
import PokemonTeam from './PokemonTeam.vue'
export default {
    components: {
        PokemonList,
        PokemonTeam,
        HelloWorld
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
        },
        seeList(){
            this.showTeam = false;
        }
    }
}
</script>

<style></style>