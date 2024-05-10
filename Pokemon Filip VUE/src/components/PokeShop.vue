<template>
    <div v-if="items">
        <h2 style="margin-top: 95px;">Poke Shop</h2>
        <div class="pokemon-card-container">
            <div class="pokemon-card" v-for="item in items" :key="item.name">
                <div class="pokemon-header">
                    <h2 class="pokemon-name">{{ item.name }}</h2>
                    <h3 class="pokemon-id"> {{ item.cost }}</h3>
                    <!-- <h2 class="addFavorite" @click="addFavorite(item)">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="currentColor"
                            class="bi bi-heart" viewBox="0 0 16 16">
                            <path
                                d="m8 2.748-.717-.737C5.6.281 2.514.878 1.4 3.053c-.523 1.023-.641 2.5.314 4.385.920 1.815 2.834 3.989 6.286 6.357 3.452-2.368 5.365-4.542 6.286-6.357.955-1.886.838-3.362.314-4.385C13.486.878 10.4.28 8.717 2.01zM8 15C-7.333 4.868 3.279-3.04 7.824 1.143q.09.083.176.171a3 3 0 0 1 .176-.17C12.72-3.042 23.333 4.867 8 15" />
                        </svg>
                    </h2> -->
                </div>
                <div class="pokemon-image">
                    <img :src="item.imageUrl" :title="item.altText">
                </div>
                <!-- <div class="pokemon-details">
                    <p class="pokemon-type">Type: {{ pokemon.type }}</p>
                    <button class="add-to-team-button" @click="addToTeam(pokemon)">Add to Team</button>
                </div> -->
            </div>
        </div>
    </div>
</template>

<script>

export default {
    data() {
        return {
            items: null,
        }
    },
    mounted() {
        fetch('https://pokeapi.co/api/v2/item/')
            .then(response => response.json())
            .then(data => {
                this.items = data.results;
                return Promise.all(data.results.map(item => fetch(item.url)));
            })
            .then(responses => Promise.all(responses.map(response => response.json())))
            .then(itemData => {
                this.items.forEach((item, i) => {
                    item.name = itemData[i].name;
                    item.imageUrl = itemData[i].sprites.default;
                    item.altText = itemData[i].effect_entries[0].short_effect;
                    item.cost = itemData[i].cost;
                    // pokemon.id = itemData[index].id;
                    // pokemon.type = itemData[index].types.map(type => type.type.name).join(', ');
                    // pokemon.typeClass = this.customClasses(pokemon.type);
                    // pokemon.pokemonInFavs = true;

                })
                console.log(itemData)
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
            this.$emit('addFavorite', pokemon);
        },

    },
}


</script>
<style scoped>
@import './CardStyle.css';

</style>