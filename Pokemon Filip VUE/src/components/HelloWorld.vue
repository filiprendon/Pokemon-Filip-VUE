<template>
  <nav>
    <ul class="navBtns">
      <div>
        <li><button @click="toggle('list')">Show list</button></li>
        <li><button @click="toggle('team')">Team</button></li>
        <li><button @click="toggle('favs')">Favourites</button></li>
        <li><button @click="toggle('shop')">Poke Shop</button></li>
      </div>
      <div class="dropdown">
        <button class="btn btn-secondary" type="button" data-bs-toggle="dropdown" aria-expanded="false" title="Filter">
          <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" fill="currentColor" class="bi bi-filter-right"
            viewBox="0 0 16 16">
            <path
              d="M14 10.5a.5.5 0 0 0-.5-.5h-3a.5.5 0 0 0 0 1h3a.5.5 0 0 0 .5-.5m0-3a.5.5 0 0 0-.5-.5h-7a.5.5 0 0 0 0 1h7a.5.5 0 0 0 .5-.5m0-3a.5.5 0 0 0-.5-.5h-11a.5.5 0 0 0 0 1h11a.5.5 0 0 0 .5-.5" />
          </svg>
        </button>
        <ul class="dropdown-menu">
          <li v-for="(type, index) in pokemonTypes" :key="index">
            <a class="dropdown-item" @click="filterByType(type)">{{ type }}</a>
          </li>
        </ul>
      </div>

    </ul>
  </nav>
</template>

<script>

export default {
  data() {
    return {
      isOpen: false,
      pokemonTypes: []
    };
  },

  mounted() {
    fetch('https://pokeapi.co/api/v2/type')
      .then(response => response.json())
      .then(data => {
        const types = data.results.map(type => type.name);
        this.pokemonTypes = types;
      })
      .catch(error => console.log(error));

  },
  methods: {
    toggle(view) {
      this.$emit('toggle-view', view);
    }
  },
  toggleDropdown() {
    this.isOpen = !this.isOpen;
  },
  filterByType(type) {
    console.log(`Filtrando por tipo: ${type}`);
  }
}
</script>

<style scoped>
@import './NavBar.css';

.dropdown-item{
  text-transform: capitalize
}
</style>
