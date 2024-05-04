<template>
    <div class="pokemon-card" v-for="pokemon in pokemons" :key="pokemon.name">
        <div class="pokemon-image">
            <img :src="pokemon.imageUrl" :alt="pokemon.name">
        </div>
        <div class="pokemon-details">
            <h2 class="pokemon-name">{{ pokemon.name }}</h2>
            <p class="pokemon-description">Some quick example text to describe the Pokémon and make up the bulk of the
                card's content.</p>
            <p class="pokemon-id"></p>
            <button class="add-to-team-button">Add to Team</button>
        </div>
    </div>
</template>

<script>

export default {
    data() {
        return {
            pokemons: []
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
                    pokemon.imageUrl = pokemonData[index].sprites.front_default;
                    pokemon.id = pokemonData[index].id;

                })
                console.log(pokemonData)
            })
            .catch(error => console.log(error));
    }
    ,
    methods() {

    }
}


</script>
<style scoped>
.pokemon-card {
    display: flex;
    background-color: #f3f3f3;
    border-radius: 10px;
    margin-bottom: 20px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    /* Inicialmente, la sombra está desactivada */
    transition: transform 1.8s ease;
}

.pokemon-card:hover {
    transform: scale(1.01);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
    transition: box-shadow 1.8s ease;
}

.pokemon-image {
    flex: 1;
}

.pokemon-image img {
    width: 100%;
    border-top-left-radius: 10px;
    border-bottom-left-radius: 10px;
}

.pokemon-details {
    flex: 1;
    padding: 20px;
}

.pokemon-name {
    margin-top: 0;
    margin-bottom: 10px;
    font-size: 20px;
}

.pokemon-description {
    margin-bottom: 10px;
    font-size: 14px;
}

.add-to-team-button {
    background-color: #ccc;
    color: #fff;
    border: none;
    padding: 10px 20px;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color .5s ease;
}

.add-to-team-button:hover {
    background-color: #0056b3;
    transition: background-color .5s ease;
}
</style>