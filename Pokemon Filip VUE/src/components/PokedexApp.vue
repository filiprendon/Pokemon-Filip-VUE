<template>
    <nav>
        <ul>
            <li><button @click="showPokemonTeam">Ver Equipo</button></li>
            <li><button @click="showPokemonFavs">Ver Favoritos</button></li>
            <li><button @click="pokeShop">Poke Shop</button></li>
        </ul>
    </nav>
    <h3> {{ mensaje }}</h3>
    <div class="pokedex-container">

        <div class="content">
            <div v-if="pokemons">
                <PokemonList v-if="!showTeam && !showFavs" :pokemons="pokemons" @addTeam="addTeam"
                    @addFavorite="addFavorite" />
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
    props: {
    mensaje: String
  },
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

<style scoped>
nav {
    background-color: #4CAF50;
}

ul {
    list-style-type: none;
    margin: 0;
    padding: 0;
}

li {
    display: inline;
}

button {
    background-color: #4CAF50;
    border: none;
    color: white;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 4px 2px;
    cursor: pointer;
    padding: 10px 20px;
}

button:hover {
    background-color: #45a049;
}
</style>