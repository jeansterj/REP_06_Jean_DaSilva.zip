<template>

<h1>hello</h1>
<div class="container">

<store :item="item"></store>

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
            item: null
        }
    },
    methods: {
        fetchData() {
            fetch('https://pokeapi.co/api/v2/item-attribute/2/')
                .then(response => response.json())
                .then(data => {

                    Promise.all(data.items.map(items => fetch(items.url).then(response => response.json())))
                        .then(itemsDetails => {
                            this.item = itemsDetails;

                        })
                        .catch(err => console.log("Error fetching Item details: " + err.message));
                })
                .catch(err => console.log("Error fetching API: " + err.message))
            
        }
    },
    mounted() {
    this.fetchData();
  },
}
</script>

