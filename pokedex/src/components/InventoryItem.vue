<template>

    <h1>hello</h1>
    <div class="container">

        <div v-if="!inventary">Loading...</div>
        <div v-else>
            <div class="row mx-2">

                <div class="col-lg-2 col-md-3 col-sm-4 mb-3 me-2 text-capitalize" v-for="invent in inventary"
                    :key="invent.id">
                    <div class="card" :id="invent.id">
                        <div class="card-body">
                            <img :src="invent.sprites.default" class="card-img my-3 py-4" alt="item Image">
                            <div class="card-img-overlay">
                                <div class="text-center">
                                    <h3> {{ invent.name.replace('-', ' ') }}</h3>

                                </div>
                                <div>
                                    <div class="d-flex justify-content-around">
                                        <button @click="useItem(invent)">Use</button>
                                        <p
                                            v-if="invent.category.name == 'standard-balls' || invent.category.name == 'special-ball'">
                                            {{ maxPokeball }}</p>
                                        <p v-else>{{ maxItemsIventory }}</p>

                                        <p> / {{ invent.quantity }}</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>


            </div>

        </div>
        <store :item="item" :inventary="inventary"></store>

    </div>
</template>
<script>
import store from "./StoreItem.vue";

export default {
    components: {
        store
    },
    data() {
        return {
            item: null,
            inventary: [],
            maxPokeball: 15,
            maxItemsIventory: 5,
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
                                itemsDetails.forEach(item => {
                                    item.quantityStock = 15;
                                });
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
        useItem(item) {

            if (item.quantity > 0) {

                item.quantity -= 1
                alert(`Se ha usado el objeto ${item.name.replace('-', ' ')} del inventario`)
            }
        }
    },
    mounted() {
        this.fetchData();
    },
}
</script>
