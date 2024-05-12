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
          <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" fill="currentColor" class="bi bi-filter-right"
            viewBox="0 0 16 16">
            <path
              d="M14 10.5a.5.5 0 0 0-.5-.5h-3a.5.5 0 0 0 0 1h3a.5.5 0 0 0 .5-.5m0-3a.5.5 0 0 0-.5-.5h-7a.5.5 0 0 0 0 1h7a.5.5 0 0 0 .5-.5m0-3a.5.5 0 0 0-.5-.5h-11a.5.5 0 0 0 0 1h11a.5.5 0 0 0 .5-.5" />
          </svg>
        </button>
        <button class="btn btn-secondary" @click="toggle('inventory')"><svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" fill="currentColor"
            class="bi bi-backpack2" viewBox="0 0 16 16">
            <path d="M4.04 7.43a4 4 0 0 1 7.92 0 .5.5 0 1 1-.99.14 3 3 0 0 0-5.94 0 .5.5 0 1 1-.99-.14" />
            <path fill-rule="evenodd"
              d="M4 9.5a.5.5 0 0 1 .5-.5h7a.5.5 0 0 1 .5.5v4a.5.5 0 0 1-.5.5h-7a.5.5 0 0 1-.5-.5zm1 .5v3h6v-3h-1v.5a.5.5 0 0 1-1 0V10z" />
            <path
              d="M6 2.341V2a2 2 0 1 1 4 0v.341c2.33.824 4 3.047 4 5.659v1.191l1.17.585a1.5 1.5 0 0 1 .83 1.342V13.5a1.5 1.5 0 0 1-1.5 1.5h-1c-.456.607-1.182 1-2 1h-7a2.5 2.5 0 0 1-2-1h-1A1.5 1.5 0 0 1 0 13.5v-2.382a1.5 1.5 0 0 1 .83-1.342L2 9.191V8a6 6 0 0 1 4-5.659M7 2v.083a6 6 0 0 1 2 0V2a1 1 0 0 0-2 0M3 13.5A1.5 1.5 0 0 0 4.5 15h7a1.5 1.5 0 0 0 1.5-1.5V8A5 5 0 0 0 3 8zm-1-3.19-.724.362a.5.5 0 0 0-.276.447V13.5a.5.5 0 0 0 .5.5H2zm12 0V14h.5a.5.5 0 0 0 .5-.5v-2.382a.5.5 0 0 0-.276-.447L14 10.309Z" />
          </svg></button>

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
