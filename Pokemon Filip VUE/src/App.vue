<script>
import HelloWorld from './components/HelloWorld.vue'
import PokemonList from './components/PokemonList.vue'
import PokemonTeam from './components/PokemonTeam.vue'
import PokemonFavs from './components/PokemonFavs.vue'
import PokeShop from './components/PokeShop.vue'
import PokedexApp from './components/PokedexApp.vue'

export default{
  name: 'App',
  components:{
    HelloWorld,
    PokedexApp,
    PokemonList
  }
}
</script>

<template>
  <!-- <header>
    <HelloWorld></HelloWorld>
  </header> -->
  <div>
  <PokedexApp></PokedexApp>
  </div>
</template>

