<template>

    <h1>hello</h1>
    
    <itemStore v-for="item in items" :key="item.id" @fetchData="fetchData">
        <div class="card" :id="item.id" >
            <div class="card-body">
              <img :src="item.sprites.default" class="card-img my-3" alt="item Image">
              <div class="card-img-overlay">
                <div class="d-flex justify-content-between">

              </div>
              <div>
                <div>
                  <h2> {{ item.name }}</h2>
                </div>
              </div>
            </div>
          </div>
        </div>
    
    </itemStore>
    
    
    
    </template>
    <script>
    import itemStore from "./ItemStore.vue";
    
    export default {
        components: {
            itemStore
      },
            data(){
                return {
                  
                }
            },
            methods: {
                fetchData(){}
            }
            
        }
    </script>