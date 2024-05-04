<script>
import HelloWorld from './components/HelloWorld.vue'
import PokemonList from './components/PokemonList.vue'

export default{
  name: 'App',
  components:{
    HelloWorld,
    PokemonList
  }
}
</script>

<template>
  <div>
  <PokemonList></PokemonList>
  </div>
</template>

