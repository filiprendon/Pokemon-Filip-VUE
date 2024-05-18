<template>
    <div v-if="items">
        <h2 style="margin-top: 95px; margin-left: 150px">Poke Shop</h2>
        <div class="pokemon-card-container">
            <div class="pokemon-card" v-for="item in items" :key="item.name">
                <div class="pokemon-header">
                    <h2 class="pokemon-name">{{ item.name }}</h2>
                    <!-- <h3 class="pokemon-id"> {{ item.cost }}</h3> -->

                </div>
                <div class="pokemon-image">
                    <img :src="item.imageUrl" :title="item.altText">
                </div>
                <div class="buyBtn">
                    <button class="buy" @click="buyItem(item, 'remove')">-</button>
                    <button class="buy" @click="buyItem(item, 'buy')">+</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    props: {
        items: {
            type: Array,
            required: true,
        }
    },
    data() {
        // return {
        //     items: null,
        // }
    },
    // mounted() {
    //     fetch('https://pokeapi.co/api/v2/item')
    //         .then(response => response.json())
    //         .then(data => {
    //             this.items = data.results.map(item => ({
    //                 name: item.name,
    //                 imageUrl: null,
    //                 altText: null,
    //                 cost: null,
    //                 quantity: 0
    //             }));
    //             // Esto lo hago ya que al hacer un fetch con la url nada más me devuelve muy pocos datos
    //             // Es como hacer 'https://pokeapi.co/api/v2/item/ID' en vez de 'https://pokeapi.co/api/v2/item'
    //             return Promise.all(data.results.map(item => fetch(item.url)));
    //         })
    //         .then(responses => Promise.all(responses.map(response => response.json())))
    //         .then(itemData => {
    //             this.items.forEach((item, i) => {
    //                 item.imageUrl = itemData[i].sprites.default;
    //                 item.altText = itemData[i].effect_entries[0].short_effect;
    //                 item.cost = itemData[i].cost;
    //             })
    //         })
    //         .catch(error => console.log(error));
    // },
    methods: {
        buyItem(item, action) {
                this.$emit('addToInventory', item, action);
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