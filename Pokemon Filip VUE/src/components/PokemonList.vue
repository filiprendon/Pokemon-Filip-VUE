<template>
    <h2 style="margin-top: 95px;">Pokedex</h2>
    <div class="pokemon-card" v-for="pokemon in pokemons" :key="pokemon.name" :class="pokemon.typeClass">
        <div class="pokemon-header">
            <h2 class="pokemon-name">{{ pokemon.name }}</h2>
            <h2 class="pokemon-id">Nº {{ pokemon.id }}</h2>
            <h2 class="addFavorite" @click="addFavorite(pokemon)"><svg xmlns="http://www.w3.org/2000/svg" width="24"
                    height="24" fill="currentColor" class="bi bi-heart" viewBox="0 0 16 16">
                    <path
                        d="m8 2.748-.717-.737C5.6.281 2.514.878 1.4 3.053c-.523 1.023-.641 2.5.314 4.385.92 1.815 2.834 3.989 6.286 6.357 3.452-2.368 5.365-4.542 6.286-6.357.955-1.886.838-3.362.314-4.385C13.486.878 10.4.28 8.717 2.01zM8 15C-7.333 4.868 3.279-3.04 7.824 1.143q.09.083.176.171a3 3 0 0 1 .176-.17C12.72-3.042 23.333 4.867 8 15" />
                </svg></h2>
        </div>
        <div class="pokemon-image">
            <img :src="pokemon.imageUrl" :alt="pokemon.name">
        </div>
        <div class="pokemon-details">
            <p class="pokemon-type">Type: {{ pokemon.type }}</p>
            <button class="add-to-team-button" @click="addToTeam(pokemon)">Add to Team</button>
        </div>
    </div>

</template>

<script>

export default {
    data() {
        return {
            pokemons: [],
            team: []
        }
    },
    mounted() {
        fetch('https://pokeapi.co/api/v2/pokemon/?limit=151')
            .then(response => response.json())
            .then(data => {
                this.pokemons = data.results;
                return Promise.all(data.results.map(pokemon => fetch(pokemon.url)));
            })
            .then(responses => Promise.all(responses.map(response => response.json())))
            .then(pokemonData => {
                this.pokemons.forEach((pokemon, index) => {
                    pokemon.name = pokemonData[index].name;
                    // pokemon.imageUrl = pokemonData[index].sprites.front_default;
                    // Multiple options instead of home we can choose dream_world, official-artwork, etc...
                    pokemon.imageUrl = pokemonData[index].sprites.other.home.front_default;
                    pokemon.id = pokemonData[index].id;
                    pokemon.type = pokemonData[index].types.map(type => type.type.name).join(', ');
                    pokemon.typeClass = this.customClasses(pokemon.type)

                })
                console.log(pokemonData)
            })
            .catch(error => console.log(error));
    },
    methods: {
        addToTeam: function (pokemon) {
            this.$emit('addTeam', pokemon);
        },
        customClasses: function (type) {
            const classes = {
                'grass': type.includes('grass') || type.includes('bug'),
                'fire': type.includes('fire'),
                'water': type.includes('water'),
                'poison': type.includes('poison'),
            }
            return classes;

        }, 
        addFavorite: function (pokemon) {
            console.log(JSON.parse(JSON.stringify(pokemon)))
        },
    }
}


</script>
<style scoped>

@import './CardStyle.css';

</style>