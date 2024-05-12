<template>
  <div v-if="inventoryItems">
    <h2 style="margin-top: 95px; margin-left: 150px">Inventario</h2>
    <div class="pokemon-card-container">
      <div class="pokemon-card" v-for="item in inventoryItems" :key="item.name">
        <div class="pokemon-header">
          <h2 class="pokemon-name">{{ item.name }}</h2>
          <h3 class="pokemon-id">{{ item.quantity }}</h3>
        </div>
        <div class="pokemon-image">
          <img :src="item.imageUrl" :title="item.altText">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    inventoryItems: {
      type: Array,
      required: true
    }
  },
  mounted() {
    console.log(this.inventoryItems)
  }
};
</script>

<style scoped>
@import './CardStyle.css';
</style>
