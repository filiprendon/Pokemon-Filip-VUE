<template>
    <nav>
        <ul>
            <li><button @click="toggleView('list')">Show list</button></li>
            <li><button @click="toggleView('team')">Team</button></li>
            <li><button @click="toggleView('favs')">Favourites</button></li>
            <li><button @click="toggleView('shop')">Poke Shop</button></li>
        </ul>
    </nav>
    <div class="pokedex-container">

        <div class="content">
            <div v-if="pokemons">
                <PokemonList v-if="showList" :pokemons="pokemons" @addTeam="addTeam" @addFavorite="addFavorite" />
                <PokemonTeam v-else-if="showTeam" :team="team"></PokemonTeam>
                <PokemonFavs v-else-if="showFavs" :favs="favs"></PokemonFavs>
                <PokeShop v-else-if="showShop" :items="items"></PokeShop>
            </div>
        </div>
    </div>
</template>

<script>
import HelloWorld from './HelloWorld.vue'
import PokemonList from './PokemonList.vue'
import PokemonTeam from './PokemonTeam.vue'
import PokemonFavs from './PokemonFavs.vue'
import PokeShop from './PokeShop.vue'
export default {
    props: {
        mensaje: String
    },
    components: {
        PokemonList,
        PokemonTeam,
        PokemonFavs,
        HelloWorld,
        PokeShop
    },
    data() {
        return {
            pokemons: [],
            team: [],
            favs: [],
            showTeam: false,
            showFavs: false,
            showShop: false,
            showList: true,
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
                console.log(pokemon, ' Removed from favorites');
                PokemonList.pokemonInFavs = false;
            } else {
                this.favs.push(pokemon);
                console.log(pokemon, ' Added to favorites');
                PokemonList.pokemonInFavs = true;
            }
        },

        toggleView(view) {
            this.showList = view === 'list';
            this.showTeam = view === 'team';
            this.showFavorites = view === 'favs';
            this.showShop = view === 'shop';
        },

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