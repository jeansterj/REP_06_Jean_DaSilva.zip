<template>

    <div class="card" :id="item.id">
        <div class="card-body">
            <img :src="item.sprites.default" class="card-img my-3 py-4" alt="item Image">
            <div class="card-img-overlay">
                <div class="text-center">
                    <h3 > {{ item.name.replace('-',' ') }}</h3>

                </div>
                <div>
                    <p>Stock {{ item.quantityStock }}</p>

                    <div class="d-flex justify-content-around">

                        <button @click="decrement(item)">-</button>
                        <p> / {{ seleccion }}</p>
                        <button @click="increase(item)">+</button>

                    </div>
                </div>
            </div>
        </div>
    </div>

</template>
<script>

export default {

    props: {
        item: {
            type: Object,
            required: true
        }
    },
    
    data(){
        return{

            seleccion : 0
        }
    }
    
    ,  methods: {
        increase(item){
            if (this.seleccion != item.quantityStock) {
                this.seleccion += 1
                this.$emit("quantityChange", { item, quantity: 1 }); // Emitiendo cambio de cantidad


            }
        },
        decrement(item){
            if(this.seleccion > 0){
                this.seleccion -= 1
                this.$emit("quantityChange", { item, quantity: -1 }); // Emitiendo cambio de cantidad

            }

        }

    } 

}
</script>
