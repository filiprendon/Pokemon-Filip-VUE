<template>
    <h2 v-if="favs.length === 0" style="margin-top: 95px; margin-left: 150px">No tienes Pokémons favoritos</h2>
    <h2 v-else style="margin-top: 95px; margin-left: 150px">Favourite Pokémons</h2>
    <div class="pokemon-card-container">
        <div class="pokemon-card" v-for="pokemon in favs" :key="pokemon.name" :class="pokemon.typeClass">
            <div class="pokemon-header">
                <h2 class="pokemon-name">{{ pokemon.name }}</h2>
                <h2 class="pokemon-id">Nº {{ pokemon.id }}</h2>
                <h2 class="addFavorite" @click="addFavorite(pokemon)">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="currentColor"
                        class="bi bi-heart-fill" viewBox="0 0 16 16">
                        <path fill-rule="evenodd"
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
</template>

<script>
export default {
    data() {
        return {

        }
    },
    components: {

    },
    props: ['favs'],
    methods: {
        addFavorite: function (pokemon) {
            // actualizamos para que si se quita desde el apartado de favoritos se actualize en la lista
            pokemon.isFavorite = !pokemon.isFavorite;
            this.$emit('addFavorite', pokemon);
        },
        addToTeam: function (pokemon) {
            this.$emit('addTeam', pokemon)
        },
        removeFromTeam: function (pokemon) {
            this.$emit('removeTeam', pokemon)
        },
    }
}
</script>

<style scoped>
@import './CardStyle.css';

.addFavorite:hover {
    font-family: 'bootstrap-icons';
    cursor: pointer;
    content: "\f771";
}
</style>