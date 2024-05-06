<template>

    <div> 
        <h1 class="text-white">Filtrar</h1>

        <div class="slider-container d-flex justify-content-center">
            <div class="mx-4">
                <input type="range" min="1" :max="maxRange" v-model="minRange" id="myRange" class="custom-range" >
                <p class="text-white">Desde el Pokémon #{{ minRange }} </p>
            </div>
            <div class="mx-4">
                <input type="range" :min="minRange" max="151" v-model="maxRange" class="custom-range" >
                <p class="text-white">Hasta el Pokémon #{{ maxRange }}</p>
           </div>
           <button type="submit" @click="filterPokemonRange(minRange,maxRange)">Filtrar</button>
        </div>
    </div>

    
    </template>
    <script>
    export default {
        data(){
            return {
                minRange: 1,
                maxRange: 151
            }
        },
        methods: {
            filterPokemonRange(minRange,maxRange){

            this.$emit('filterPokemonRange',minRange,maxRange)
           }
        }
    }
    </script>
    <style>
    .custom-range {
        -webkit-appearance: none;
        width: 100%;
        height: 10px;
        background: white; 
        outline: none;
        opacity: 0.9;
        -webkit-transition: .2s;
        transition: opacity .2s;
        border-radius: 5px; 
    }
    
    .custom-range::-webkit-slider-thumb {
        -webkit-appearance: none;
        appearance: none;
        width: 25px; 
        height: 25px; 
        background: gold; 
        cursor: pointer;
        border-radius: 50%; 
    }
    
    .custom-range::-moz-range-thumb {
        width: 25px; 
        height: 25px; 
        background: gold;
        cursor: pointer;
        border-radius: 50%; 
    }
    </style>