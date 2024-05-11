<template>
  <nav>
    <ul>
      <li><button @click="toggle('list')">Show list</button></li>
      <li><button @click="toggle('team')">Team</button></li>
      <li><button @click="toggle('favs')">Favourites</button></li>
      <li><button @click="toggle('shop')">Poke Shop</button></li>
    </ul>
  </nav>
</template>

<script>
export default {

  methods: {
    toggle(view) {
      this.$emit('toggle-view', view);
    }
  }
}
</script>

<style scoped>
@import './NavBar.css';
</style>
