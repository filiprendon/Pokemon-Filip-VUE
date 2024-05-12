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
        <button class="btn btn-secondary" type="button" data-bs-toggle="dropdown" aria-expanded="false" title="Filter"
          @click="toggleDropdown">
          <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" fill="currentColor" class="bi bi-filter-right"
            viewBox="0 0 16 16">
            <path
              d="M14 10.5a.5.5 0 0 0-.5-.5h-3a.5.5 0 0 0 0 1h3a.5.5 0 0 0 .5-.5m0-3a.5.5 0 0 0-.5-.5h-7a.5.5 0 0 0 0 1h7a.5.5 0 0 0 .5-.5m0-3a.5.5 0 0 0-.5-.5h-11a.5.5 0 0 0 0 1h11a.5.5 0 0 0 .5-.5" />
          </svg>
        </button>
        <button @click="nword">OK</button>

        <ul class="dropdown-menu" @click.stop>
          <li v-for="(type, index) in pokemonTypes" :key="index">
            <label class="dropdown-item">
              <input type="checkbox" @click="filterByType(type)">
              {{ type }}
            </label>
          </li>
        </ul>
      </div>

    </ul>
  </nav>
</template>

<script>
import { computed } from 'vue';


export default {
  props: {
    pokemons: {
      type: Array,
      required: true
    },
    team: {
      type: Array,
      required: true
    },
  },

  data() {
    return {
      isOpen: false,
      pokemonTypes: []
    };
  },

  methods: {
    toggle(view) {
      if (this.team.length < 6 && view === 'team') {
        alert('Puedes ver tu equipo cuando tengas 6 Pokémons, actualmente tienes ' + this.team.length);
        return;
      } else {
        this.$emit('toggle-view', view);
      }

    },

    toggleDropdown() {
      this.pokemonTypes = [...new Set(this.pokemons.map(pokemon => pokemon.type))];
      this.isOpen = !this.isOpen;
    },
    filterByType(type) {
      console.log(`Filtrando por tipo: ${type}`);
    },
  },
  computed: {
    pokemonsFiltrados() {
      if (this.pokemonTypes.length === 0) {
        return this.pokemons;
      } else {
        return this.pokemons.filter(pokemon =>
          this.pokemonTypes.includes(pokemon.type)
        );
      }
    }
  },
};
</script>


<style scoped>
@import './NavBar.css';

.dropdown-item {
  text-transform: capitalize;
  cursor: pointer;
}
</style>
