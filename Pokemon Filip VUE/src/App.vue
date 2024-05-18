<template>
  <!-- <nav>
      <ul>
          <li><button @click="toggleView('list')">Show list</button></li>
          <li><button @click="toggleView('team')">Team</button></li>
          <li><button @click="toggleView('favs')">Favourites</button></li>
          <li><button @click="toggleView('shop')">Poke Shop</button></li>
      </ul>
  </nav> -->
  <NavComp @toggle-view="toggleView" :pokemons="pokemons" :team="team" @filter="handleCheckbox"
    @filterSearch="filterSearch" @searchFromRange="searchFromRange" :typesForFilter />
  <div class="pokedex-container">
    <div class="content">
      <div v-if="pokemons">
        <PokemonList v-if="showList || showTeam && team.length < 6" :pokemonInFavs="pokemonInFavs" :pokemons="pokemons"
          :pokemonFilter="filteredPokemons" @addTeam="addTeam" @addFavorite="addFavorite" @removeTeam="removeTeam"
          @filterType="filterType" :typesForFilter />
        <PokemonTeam v-else-if="showTeam && team.length >= 6" :team="team" @addFavorite="addFavorite"
          @removeTeam="removeTeam"></PokemonTeam>
        <PokemonFavs v-else-if="showFavs" :favs="favs" @addFavorite="addFavorite" @addTeam="addTeam"
          @removeTeam="removeTeam"></PokemonFavs>
        <PokeShop v-else-if="showShop" :items @addToInventory="addInventory"></PokeShop>
        <Inventory v-else-if="showInventory" :items :inventoryItems="inventoryItems"></Inventory>
      </div>
    </div>
  </div>
</template>

<script>

import NavComp from './components/NavComp.vue'
import PokemonList from './components/PokemonList.vue'
import PokemonTeam from './components/PokemonTeam.vue'
import PokemonFavs from './components/PokemonFavs.vue'
import PokeShop from './components/PokeShop.vue'
import Inventory from './components/Inventory.vue'
import RangeSlider from './components/RangeSlider.vue'
export default {
  props: {
    mensaje: String,
  },
  components: {
    PokemonList,
    PokemonTeam,
    PokemonFavs,
    NavComp,
    PokeShop,
    Inventory,
    RangeSlider,
  },
  data() {
    return {
      pokemons: [],
      team: [],
      favs: [],
      inventoryItems: [],
      items: [],
      showTeam: false,
      showFavs: false,
      showShop: false,
      showList: true,
      showInventory: false,
      pokemonInFavs: false,
      filterType: '',
      searchRange: [],
      typesForFilter: []
      // pokemonInTeam: false,
    }
  },
  mounted() {
    fetch('https://pokeapi.co/api/v2/pokemon/?limit=151')
      .then(response => response.json())
      .then(data => {
        this.pokemons = data.results;
        return Promise.all(data.results.map(pokemon => fetch(pokemon.url)));
      })
      .then(responses => Promise.all(responses.map(response => response.json())))
      .then(pokemonData => {
        const uniqueTypesSet = new Set();

        this.pokemons.forEach((pokemon, index) => {
          pokemon.name = pokemonData[index].name;
          pokemon.imageUrl = pokemonData[index].sprites.other.home.front_default;
          pokemon.id = pokemonData[index].id;
          pokemon.types = pokemonData[index].types.map(type => type.type.name);

          // por cada tipo lo añado al set de tipo unicos para usarlo en el filtro
          pokemon.types.forEach(type => uniqueTypesSet.add(type));
          pokemon.type = pokemon.types.join(', ');
          pokemon.typeClass = this.customClasses(pokemon.type);
        });

        this.typesForFilter = [...uniqueTypesSet];

      })
      .catch(error => console.log(error));

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
        });
      })
      .catch(error => console.log(error));
  },


  computed: {
    // Esta parte se encarga de la lista y de las busquedas
    filteredPokemons() {
      if (this.filterType && this.searchRange.length > 0) {
        let tiposLimpios = this.filterType.join(', ');
        let [minVal, maxVal] = this.searchRange;

        return this.pokemons.filter(pokemon => {
          let tiposArray = tiposLimpios.split(', ');
          return tiposArray.some(tipo => pokemon.type.includes(tipo)) &&
            pokemon.id >= minVal && pokemon.id <= maxVal;
        });
      } else if (this.filterType) {
        let tiposLimpios = this.filterType.join(', ');
        return this.pokemons.filter(pokemon => {
          let tiposArray = tiposLimpios.split(', ');
          return tiposArray.some(tipo => pokemon.type.includes(tipo));
        });
      } else if (this.searchRange.length > 0) {
        let [minVal, maxVal] = this.searchRange;
        return this.pokemons.filter(pokemon => {
          return pokemon.id >= minVal && pokemon.id <= maxVal;
        });
      } else {
        return this.pokemons;
      }
    }

  },
  methods: {
    customClasses: function (type) {
      const classes = {
        'grass': type.includes('grass') || type.includes('bug'),
        'fire': type.includes('fire'),
        'water': type.includes('water'),
        'poison': type.includes('poison'),
      }
      return classes;

    },
    filterSearch(type) {
      this.showList = true;
      this.filterType = type;
      let filtros = this.filterType.join(', ')
      console.log(filtros)
    },
    searchFromRange(minVal, maxVal) {
      this.showTeam = true;
      this.searchRange = [minVal, maxVal];
      console.log(`The min val is ${minVal} and the max val is ${maxVal}`);

      // this.searchRange = [minVal, maxVal];
      // console.log(this.searchRange)

    },
    addTeam(pokemon) {
      // const pokemonInTeam = this.team.some(p => p.id === pokemon.id);
      if (this.team.length == 6) {
        alert("You've reached the maximum amount of Pokémons in your team, to add another one, please remove a Pokémon from your current team")
      } else {
        // actualizo aquí, si lo hago en la funcion addToTeam se cambia el boton aunque salga el mensaje
        pokemon.inTeam = !pokemon.inTeam;
        this.team.push(pokemon);
      }


    },
    removeTeam(pokemon) {
      const pokemonIndex = this.team.findIndex(p => p.id === pokemon.id);
      if (confirm('Are you sure you want to remove this Pokémon?')) {
        if (pokemonIndex !== -1) {
          pokemon.inTeam = !pokemon.inTeam;
          this.team.splice(pokemonIndex, 1);
          console.log(pokemon, ' Removed from team');
        } else {
          return;
        }
      }
    },
    addFavorite(pokemon) {
      const pokemonIndex = this.favs.findIndex(p => p.id === pokemon.id);
      if (pokemonIndex !== -1) {
        this.favs.splice(pokemonIndex, 1);
        console.log(pokemon, ' Removed from favorites');
        this.pokemonInFavs = false;
      } else {
        this.favs.push(pokemon);
        console.log(pokemon, ' Added to favorites');
        this.pokemonInFavs = true;
      }
    },

    toggleView(view) {
      this.showList = view === 'list';
      this.showTeam = view === 'team';
      this.showFavs = view === 'favs';
      this.showShop = view === 'shop';
      this.showInventory = view === 'inventory';
    },
    addInventory(item, action) {
      // console.log(item.name + ' ' + action);

      // Verificar si el item ya está en el inventario
      let existingItemIndex = this.items.findIndex(inventoryItem => inventoryItem.name === item.name);

    if (existingItemIndex !== -1) {
        if (this.items[existingItemIndex].quantity == 0 && action == 'remove') {
            alert('Has alcanzado el máximo');
            return;
        }
        if (this.items[existingItemIndex].quantity == 4 && action == 'buy') {
            alert('Has alcanzado el máximo');
            return;
        }
        if (action == 'buy') {
            console.log('Item is already in the inventory. Updating quantity...');
            this.items[existingItemIndex].quantity += 1;
        } else if (action == 'remove') {
            console.log('Item is already in the inventory. Updating quantity...');
            this.items[existingItemIndex].quantity -= 1;
        }
    } else {
        console.log('Item added to inventory.');
        this.items.push({ ...item, quantity: 1 });
    }




      console.log(this.inventoryItems)
      // const existingItemIndex = this.inventoryItems.findIndex(i => i.id === item.id);
      // if (existingItemIndex !== -1) {
      //   this.inventoryItems[existingItemIndex].quantity += item.quantity;
      //   console.log('Quantity updated for:', item.name);
      // } else {
      //   this.inventoryItems.push(item);
      //   console.log('Item added:', item.name);
      // }
    },
    addToFilterTypes(pokemon) {
      console.log(pokemon.type)

    }




  }
}
</script>

<style scoped>
@import './components/CardStyle.css';
</style>