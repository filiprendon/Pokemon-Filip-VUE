<script>
import HelloWorld from './components/HelloWorld.vue'
import PokemonList from './components/PokemonList.vue'
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
  <header>
    <HelloWorld></HelloWorld>
  </header>
  <div>
  <PokedexApp></PokedexApp>
  </div>
</template>

