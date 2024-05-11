<template>
    <div v-if="items">
        <h2 style="margin-top: 95px;">Poke Shop</h2>
        <div class="pokemon-card-container">
            <div class="pokemon-card" v-for="item in items" :key="item.name">
                <div class="pokemon-header">
                    <h2 class="pokemon-name">{{ item.name }}</h2>
                    <h3 class="pokemon-id"> {{ item.cost }}</h3>

                </div>
                <div class="pokemon-image">
                    <img :src="item.imageUrl" :title="item.altText">
                </div>
                <div class="buyBtn">
                    <button class="buy" @click="buyItem(item.name, -1)">-</button>
                    <button class="buy" @click="buyItem(item.name, 1)">+</button>
                </div>
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
        fetch('https://pokeapi.co/api/v2/item')
            .then(response => response.json())
            .then(data => {
                this.items = data.results.map(item => ({
                    name: item.name,
                    imageUrl: null,
                    altText: null,
                    cost: null,
                    quantity: 0
                }));
                return Promise.all(data.results.map(item => fetch(item.url)));
            })
            .then(responses => Promise.all(responses.map(response => response.json())))
            .then(itemData => {
                this.items.forEach((item, i) => {
                    item.imageUrl = itemData[i].sprites.default;
                    item.altText = itemData[i].effect_entries[0].short_effect;
                    item.cost = itemData[i].cost;
                })
            })
            .catch(error => console.log(error));
    }
    ,
    methods: {
        buyItem(name, total) {
            const itemIndex = this.items.findIndex(item => item.name === name);
            if (itemIndex !== -1) {
                this.items[itemIndex].quantity += total;
                console.log(`Cantidad ${name}: ${this.items[itemIndex].quantity}`);
            }
        }

    },
}


</script>
<style scoped>
@import './CardStyle.css';

.buyBtn {
    display: flex;
    justify-content: space-between;
    padding: 2px 15px;
}
</style>