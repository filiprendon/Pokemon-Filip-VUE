<template>
  <div>
    <label for="range">Select range:</label>
    <input type="range" id="range" name="range" min="1" max="151" v-model="minValue">
    <input type="range" id="range2" name="range2" min="1" max="151" v-model="maxValue">
    <p>Min: {{ minValue }}</p>
    <p>Max: {{ maxValue }}</p>
    <button @click="searchPokemons(minValue, maxValue)">Search</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      minValue: 1,
      maxValue: 151
    };
  },
  methods: {
    searchPokemons(minVal, maxVal) {
      // Los convierto a números pq hay veces donde me sale la alerta aunque el max no sea inferior
      minVal = parseInt(minVal);
      maxVal = parseInt(maxVal);
      if (minVal > maxVal) {
        alert('The max value cannot be lower than the min value');
      } else {
        // console.log(`The min val is ${minVal} and the max val is ${maxVal}`)
        this.$emit('searchRange', minVal, maxVal)
      }
    }
  },
};
</script>

<style scoped>
/* Personaliza el estilo del rango deslizante según tus preferencias */
input[type=range] {
  -webkit-appearance: none;
  width: 100%;
  height: 10px;
  background: #d3d3d3;
  outline: none;
  opacity: 0.7;
  -webkit-transition: .2s;
  transition: opacity .2s;
}

/* Estilo del rango deslizante en su estado "hover" */
input[type=range]:hover {
  opacity: 1;
}

/* Estilo del control deslizante "thumb" */
input[type=range]::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 25px;
  height: 25px;
  background: #4CAF50;
  cursor: pointer;
}

/* Estilo del control deslizante "thumb" en su estado "hover" */
input[type=range]::-webkit-slider-thumb:hover {
  background: #45a049;
}

/* Estilo del control deslizante "thumb" en su estado "activo" */
input[type=range]::-webkit-slider-thumb:active {
  background: #45a049;
}
</style>
