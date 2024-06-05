<template>
    <div v-if="items">
        <h2 style="margin-top: 95px; margin-left: 150px">Poke Shop</h2>
        <div class="pokemon-card-container">
            <div class="pokemon-card" v-for="item in items" :key="item.name">
                <div class="pokemon-header">
                    <h2 class="pokemon-name">{{ item.name }}</h2>
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