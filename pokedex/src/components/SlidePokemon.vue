<template>

<div> 
    <div class="slider-container">
        <input type="range" min="1" max="151" v-model="selectedRange">
        <p>Desde el Pokémon #{{ selectedRange[0] }} hasta el Pokémon #{{ selectedRange[1] }}</p>
    </div>


</div>


</template>
<script>
export default {
data(){
return {

    selectedRange: [1, 151] // Rango inicial, del primer al último Pokémon

}

},
methods: {



}

}


</script>
