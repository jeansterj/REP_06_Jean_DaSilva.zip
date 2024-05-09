<template>

    <h1>hello</h1>
    <div v-if="!inventary">Loading...</div>
    <div v-else>
        <div class="container">
            <div class="row mx-2">


                <itemStore class="col-lg-2 col-md-3 col-sm-4 mb-3 me-2" v-for="inventary in inventary"
                    :key="inventary.id" :inventary="inventary" >
                </itemStore>
            </div>
        </div>
    </div>
    <store @buyItem="buyItem"></store>

</template>
<script>
import store from "./StoreItem.vue";
import itemStore from "./ItemStore.vue";

export default {
    components: {
        store,
        itemStore
    },
    data() {
        return {
            item: null,
            inventary: [],
            categoryItem: 'standard-balls'
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
                                this.item = itemsDetails;
                                this.addInventory(this.item)
                            }
                        })
                        .catch(err => console.log("Error fetching Item details: " + err.message))
                })
                .catch(err => console.log("Error fetching API: " + err.message));
        },

        addInventory(item) {
            if (item !== null && item.length > 0) {
                item.forEach(items => {
                    if (items.category && (items.category.name === 'standard-balls' || items.category.name === 'healing' || items.category.name === 'pp-recovery')) {
                        if (items.category.name == 'standard-balls' || items.category.name == 'special-ball') {
                            items.quantity = 5
                        } else {
                            items.quantity = 2

                        }
                        this.inventary.push(items);
                    }
                });
            }
        },
    buyItem(itemData) {
        console.log(itemData.id)
        console.log(this.inventary)

      const existingItem = this.inventary.find(item => item.id === itemData.id);
      console.log(existingItem)

      if (existingItem) {
        existingItem.quantity += itemData.selectedQuantity;
      } else {
        this.inventary.push(itemData);
      }
      itemData.selectedQuantity= 0
    }
        

    },
    mounted() {
        this.fetchData();
    },
}
</script>
