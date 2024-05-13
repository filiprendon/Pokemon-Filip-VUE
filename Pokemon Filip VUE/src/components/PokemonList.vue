<template>
    <div v-if="pokemonFilter">
        <h2 style="margin-top: 95px; margin-left: 150px">Pokedex</h2>
        <div class="pokemon-card-container">
            <div class="pokemon-card" v-for="pokemon in pokemonFilter" :key="pokemon.name" :class="pokemon.typeClass">
                <div class="pokemon-header">
                    <h2 class="pokemon-name">{{ pokemon.name }}</h2>
                    <h2 class="pokemon-id">Nº {{ pokemon.id }}</h2>
                    <h2 class="addFavorite" @click="addToFavorite(pokemon)">
                        <svg :class="{ 'bi-heart': !pokemon.isFavorite, 'bi-heart-fill': pokemon.isFavorite }"
                            xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="currentColor"
                            viewBox="0 0 16 16">
                            <path v-if="!pokemon.isFavorite"
                                d="m8 2.748-.717-.737C5.6.281 2.514.878 1.4 3.053c-.523 1.023-.641 2.5.314 4.385.920 1.815 2.834 3.989 6.286 6.357 3.452-2.368 5.365-4.542 6.286-6.357.955-1.886.838-3.362.314-4.385C13.486.878 10.4.28 8.717 2.01zM8 15C-7.333 4.868 3.279-3.04 7.824 1.143q.09.083.176.171a3 3 0 0 1 .176-.17C12.72-3.042 23.333 4.867 8 15" />
                            <path v-else fill-rule="evenodd"
                                d="M8 1.314C12.438-3.248 23.534 4.735 8 15-7.534 4.736 3.562-3.248 8 1.314" />
                        </svg>
                    </h2>
                </div>
                <div class="pokemon-image">
                    <img :src="pokemon.imageUrl" :alt="pokemon.name">
                </div>
                <div class="pokemon-details">
                    <p class="pokemon-type">Type: {{ pokemon.type }}</p>
                    <button v-if="!pokemon.inTeam" class="add-to-team-button" @click="addToTeam(pokemon)">Add to
                        Team</button>
                    <button v-else class="remove-from-team-button" @click="removeFromTeam(pokemon)">Remove From
                        Team</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import NavComp from './NavComp.vue';


export default {
    props: {
        pokemons: {
            type: Array,
            required: true,
        },
        pokemonInFavs: {
            type: Boolean,
            required: true,
        },
        pokemonFilter:{
            type: String,
            required: true,
        },
        filterType: {
            type: String,
            required: true,
        }
    },
    computed: {
        filteredPokemons() {
            return this.pokemons.filter(pokemon => pokemon.type.includes('grass'));
        }
    },

    data() {
        return {
            // pokemons: [],
            team: [],
            favs: [],
        }
    },
    methods: {
        addToTeam: function (pokemon) {

            this.$emit('addTeam', pokemon);
        },
        removeFromTeam: function (pokemon) {
            this.$emit('removeTeam', pokemon)
        },
        addToFavorite: function (pokemon) {
            pokemon.isFavorite = !pokemon.isFavorite;
            this.$emit('addFavorite', pokemon);
        },
        filterType(type) {
            return this.pokemons.filter(pokemon => pokemon.type.includes(type));
        }
    }
}


</script>
<style scoped>
@import './CardStyle.css';
</style>