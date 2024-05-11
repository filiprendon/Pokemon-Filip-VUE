<template>
    <div>
      <h3>Inventory</h3>
      <div v-for="item in inventoryItems" :key="item.name">
        {{ item.name }} - Quantity: {{ item.quantity }}
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      items: {
        type: Array,
        required: true
      }
    },
    data() {
      return {
        inventoryItems: []
      };
    },
    created() {
      // Inicializar el inventario con la cantidad de cada artículo que el jugador tiene
      this.items.forEach(item => {
        this.inventoryItems.push({ name: item.name, quantity: 0 });
      });
    },
    methods: {
      // Método para actualizar la cantidad de un artículo en el inventario
      updateQuantity(itemName, quantity) {
        const itemIndex = this.inventoryItems.findIndex(item => item.name === itemName);
        if (itemIndex !== -1) {
          this.inventoryItems[itemIndex].quantity += quantity;
        }
      }
    },
    watch: {
      // Vigilar los cambios en el inventario para actualizar la cantidad de los artículos
      items: {
        handler(newItems) {
          newItems.forEach((item, index) => {
            this.inventoryItems[index].name = item.name;
            // Si el artículo es nuevo, añadirlo al inventario con una cantidad inicial de 0
            if (index >= this.inventoryItems.length) {
              this.inventoryItems.push({ name: item.name, quantity: 0 });
            }
          });
        },
        deep: true
      }
    }
  };
  </script>
  