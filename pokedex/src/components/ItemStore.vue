<template>

    <div class="card" :id="item.id">
        <div class="card-body">
            <img :src="item.sprites.default" class="card-img my-3 py-4" alt="item Image">
            <div class="card-img-overlay">
                <div class="text-center">
                    <h3 > {{ item.name }}</h3>

                </div>
                <div>
                    <div class="d-flex justify-content-around">
                        <button>-</button>
                        <p>disponible / seleccionado</p>
                        <button>+</button>

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
    }

}
</script>
