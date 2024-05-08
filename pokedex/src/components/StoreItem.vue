<template>

    <h1>hello</h1>
    <div class="row mx-2">


    <itemStore class="col-lg-2 col-md-3 col-sm-4 mb-3 me-2" v-for="item in item" :key="item.id" :item="item"  @quantityChange="handleQuantityChange"></itemStore>
    </div>

    <button @click="buySelect(item)"> Buy Selection</button>
    </template>
    <script>
    import itemStore from "./ItemStore.vue";
    
    export default {
        components: {
            itemStore
      },props:{
        item:{
            type:Object,
            required: true
        },
        inventary:{
            type: Array,
            required: true
        }
    }   
    , methods:{
        handleQuantityChange({ item, quantity }) {
      const existingItem = this.inventory.find(i => i.item.id === item.id);
      if (existingItem) {
        existingItem.quantity += quantity;
      } else {
        this.inventory.push({ item, quantity });
      }
    },
    buySelect() {
      this.inventory.forEach(({ item, quantity }) => {
        if (quantity > 0) {
          item.quantityStock -= quantity; // Actualiza la cantidad en stock
          this.$emit("addToInventory", { ...item, quantity });
        }
      });
      this.inventory = []; // Limpia el registro después de agregar al inventario
    }
  }
        }
    </script>
 