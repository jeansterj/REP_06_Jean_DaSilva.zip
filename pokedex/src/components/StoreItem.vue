<template>

    <h1>hello</h1>
    <div class="row mx-2">


    <itemStore class="col-lg-2 col-md-3 col-sm-4 mb-3 me-2" v-for="item in item" :key="item.id" :item="item"></itemStore>
    </div>
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
        }
    } 
        }
    </script>
 