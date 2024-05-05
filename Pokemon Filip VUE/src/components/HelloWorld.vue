<template>
  <nav class="navbar">
    <div class="navbar-brand">
      <h1 class="green">{{ msg }}</h1>
    </div>
    <div class="navbar-menu">
      <button @click="showTeam">Mostrar Equipo Pokémon</button>
      <!-- <PokemonTeam v-if="showTeam" :team="team" /> -->
    </div>
  </nav>
</template>

<script>
// import PokemonList from './PokemonList.vue'
// import PokemonTeam from './PokemonTeam.vue'
export default {
  props: {
    msg: String
  },

  methods: {
    showTeam: function () {
      this.$emit('showTeam')
    }
  }
}
</script>

<style scoped>
.navbar {
  background-color: rgba(55, 97, 168, .5);
  backdrop-filter: blur(15px);
  position: fixed;
  width: 100%;
  z-index: 9999;
  color: #fff;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 20px;
}

.navbar-brand {
  font-size: 1.5rem;
}

.navbar-menu {
  display: flex;
}

.navbar-item {
  margin-right: 20px;
  color: #fff;
  text-decoration: none;
  font-size: 1.2rem;
}

.navbar-item:hover {
  text-decoration: underline;
}
</style>
