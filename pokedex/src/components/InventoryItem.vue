<template>
    <h1>hello</h1>
    <div v-if="!inventary">Loading...</div>
    <div v-else>
        <div class="container">
            <div class="row mx-2">
                <itemStore class="col-lg-2 col-md-3 col-sm-4 mb-3 me-2" v-for="inventaryItem in inventary"
                    :key="inventaryItem.id" :inventary="inventaryItem">
                </itemStore>
            </div>
        </div>
    </div>
    <StoreItem @buySelection="buySelection"></StoreItem>
</template>

<script>
import itemStore from "./ItemStore.vue";
import StoreItem from "./StoreItem.vue";

export default {
    components: {
        StoreItem,
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
            const limitItem = 50
            fetch('https://pokeapi.co/api/v2/item/?limit=' + limitItem)
                .then(response => response.json())
                .then(data => {
                    Promise.all(data.results.map(items => fetch(items.url).then(response => response.json())))
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
        buySelection(itemData) {

            itemData.forEach(item => {
                const existingItem = this.inventary.find(existing => existing.id === item.id);

                if (existingItem) {

                    existingItem.quantity += item.quantity;

                    if (existingItem.category && (existingItem.category.name === 'standard-balls' || existingItem.category.name === 'healing' || existingItem.category.name === 'pp-recovery')) {
                        if (existingItem.category.name == 'standard-balls' || existingItem.category.name == 'special-ball') {
                            if (existingItem.quantity > 15) {
                                existingItem.quantity = 15
                            }
                        } else {
                            if (existingItem.quantity > 15) {
                                existingItem.quantity = 5
                            }                        
                        }
                    }

                } else {
                    this.inventary.push(item);
                }
            });

        }
    },
    mounted() {
        this.fetchData();
    },
}
</script>
