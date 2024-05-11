<template>

  <h1 class="text-white my-5">Items available in store</h1>

  <div class="container-fluid">
    <div v-if="!item">Loading...</div>
    <div v-else>
      <div class="row mx-2 d-flex justify-content-center">

        <div class="col-lg-2 col-md-3 col-sm-4 mb-3 me-2 cardAjust" v-for="itemData in item" :key="itemData.id">
          

          <div class="card" :id="itemData.id">
            <div class="card-body text-capitalize">
              <img :src="itemData.sprites && itemData.sprites.default ? itemData.sprites.default : 'fallback-image-url'" class="card-img my-5" alt="item Image">
              <div class="card-img-overlay">
                <div class="text-center">
                  <h3> {{ itemData.name.replace('-', ' ') }}</h3>
                  <p v-if="itemData.quantityStock <= 0">Out of stock</p>
                  <p v-else class="mb-5">Stock {{ itemData.quantityStock }}</p>
                </div>
                <div class="buttonAjust">
                
                <div class="d-flex justify-content-around">
                  <!-- Deshabilita el botón si el stock máximo se ha alcanzado -->
                  <button @click="decrement(itemData)" :disabled="itemData.quantityStock <= 0" class="btn btn-danger">-</button>
                  <p>{{ itemData.selectedQuantity }}</p>
                  <button @click="increase(itemData)" :disabled="itemData.quantityStock <= 0" class="btn btn-info">+</button>

                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <button class="btn btn-success my-4" @click="buySelection()">Buy Selection</button>
    </div>
  </div>

</template>
<script>

export default {
  emits: ['buySelection'],
  data() {
    return {
      item: null,
      seleccion: 0,
      selectedItems: []


    }
  },
  methods: {
    fetchData() {
      const limitItem = 50
      fetch('https://pokeapi.co/api/v2/item/?limit=' + limitItem)
        .then(response => response.json())
        .then(data => {
          Promise.all(data.results.map(items => fetch(items.url).then(response => response.json())))
            .then(itemsDetails => {
              if (itemsDetails.length > 0) {
                itemsDetails.forEach(item => {
                  item.quantityStock = 15;
                  item.selectedQuantity = 0;
                });
                this.item = itemsDetails;
              }
            })
            .catch(err => console.log("Error fetching Item details: " + err.message))
        })
        .catch(err => console.log("Error fetching API: " + err.message));
    },

    increase(item) {
      if (item.selectedQuantity < item.quantityStock) {
        item.selectedQuantity += 1;
        if (!this.selectedItems.find(selectedItem => selectedItem.id === item.id)) {
          this.selectedItems.push({ id: item.id, name:item.name ,  quantity: 1 , sprites: { default: item.sprites.default } }); // Agrega el elemento seleccionado si no existe en el array
        } else {
          const selectedItem = this.selectedItems.find(selectedItem => selectedItem.id === item.id);
          selectedItem.quantity += 1; // Incrementa la cantidad si ya existe en el array
        }
      }
    },
    decrement(item) {
      if (item.selectedQuantity > 0) {
        item.selectedQuantity -= 1;
        const selectedItemIndex = this.selectedItems.findIndex(selectedItem => selectedItem.id === item.id);
        if (selectedItemIndex !== -1) {
          const selectedItem = this.selectedItems[selectedItemIndex];
          selectedItem.quantity -= 1;
          if (selectedItem.quantity === 0) {
            this.selectedItems.splice(selectedItemIndex, 1); // Elimina el elemento del array si la cantidad llega a cero
          }
        }
      }
    },
    buySelection() {
      this.$emit('buySelection', this.selectedItems); 
    // Emite los elementos seleccionados como un array de objetos {id, quantity}
    this.item.forEach(item => {
      if (item.quantityStock > 0) {
        item.quantityStock -= item.selectedQuantity
      } 
        item.selectedQuantity = 0;
      });

      this.selectedItems= []

    },
  }, mounted() {
    this.fetchData();
  }
}
</script>
<style>
.buttonAjust{
  margin-top: 55%;
}
</style>