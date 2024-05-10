<template>
    <h2 style="margin-top: 95px;">Your team</h2>
    <div v-if="team.length === 0">
        <p>You don't have Pokemons in your team.</p>
    </div>
    <div class="pokemon-card" v-for="pokemon in team" :key="pokemon.name" :class="pokemon.typeClass" v-if="team.length = 2">
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
            <button class="remove-from-team-button" @click="removeFromTeam(pokemon)">Remove from team</button>
        </div>
    </div>
</template>

<script>
export default {
    props: ['team']
}
</script>

<style scoped>

@import './CardStyle.css';

</style>