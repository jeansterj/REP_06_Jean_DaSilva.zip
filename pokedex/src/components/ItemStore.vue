<template>

    <div class="card" :id="inventary.id">
        <div class="card-body">
            <img :src="inventary?.sprites?.default" class="card-img my-3 py-4" alt="item Image">
            <div class="card-img-overlay">
                <div class="text-center">
                    <h3> {{ inventary?.name.replace('-', ' ') }}</h3>

                </div>
                <div>
                    <div class="d-flex justify-content-around ajust">
                        <button class="btn btn-primary" @click="useItem(inventary)">Use</button>
                        <p
                            v-if="inventary?.category?.name == 'standard-balls' || inventary?.category?.name == 'special-ball'">
                            {{ maxPokeball }}</p>
                        <p v-else>{{ maxItemsIventory }}</p>

                        <p> / {{ inventary.quantity }}</p>
                    </div>
                </div>
            </div>
        </div>
    </div>

</template>
<script>

export default {

    props: {
        inventary: {
            type: Object,
            required: true
        }
    },

    data() {
        return {
            maxPokeball: 15,
            maxItemsIventory: 5,
            seleccion: 0
        }
    }

    , methods: {

        useItem(item) {

            if (item.quantity > 0) {

                item.quantity -= 1
                alert(`Se ha usado el objeto ${item.name.replace('-', ' ')} del inventario`)
            }
        }
    }

}
</script>
<style>

.ajust{

    margin-top: 60%
}

</style>
