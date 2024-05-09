<template>

  <h1>hello</h1>

  <div class="container">
    <div v-if="!item">Loading...</div>
    <div v-else>
      <div class="row mx-2">

        <div class="col-lg-2 col-md-3 col-sm-4 mb-3 me-2" v-for="itemData in item" :key="itemData.id">


          <div class="card" :id="itemData.id">
            <div class="card-body">
              <img :src="itemData.sprites && itemData.sprites.default ? itemData.sprites.default : 'fallback-image-url'" class="card-img my-3 py-4" alt="item Image">
              <div class="card-img-overlay">
                <div class="text-center">
                  <h3> {{ itemData.name.replace('-', ' ') }}</h3>

                </div>
                <div>
                  <p>Stock {{ itemData.quantityStock }}</p>

                  <div class="d-flex justify-content-around">

                    <button @click="decrement(itemData)">-</button>
                    <input type="number" v-model="itemData.selectedQuantity" min="0" :max="itemData.quantityStock">
                    <button @click="increase(itemData)">+</button>
                    <button @click="addToSelection(itemData)">Add to Selection</button>

                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <button @click="buySelection()">Buy Selection</button>
    </div>
  </div>

</template>
<script>

export default {

  data() {
    return {
      item: null,
      seleccion: 0,
      selectedItems: []


    }
  },
  methods: {
    fetchData() {
      fetch('https://pokeapi.co/api/v2/item-attribute/2/')
        .then(response => response.json())
        .then(data => {
          Promise.all(data.items.map(items => fetch(items.url).then(response => response.json())))
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
      }
    },
    decrement(item) {
      if (item.selectedQuantity > 0) {
        item.selectedQuantity -= 1;
      }
    },addToSelection(item) {
      if (item.selectedQuantity > 0) {
        this.selectedItems.push({ ...item });
      }
    },
    buySelection(){
      this.$emit('buyItem',this.selectedItems)
    }
  }, mounted() {
    this.fetchData();
  }
}
</script>
