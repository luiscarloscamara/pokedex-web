<script setup>
const pokemon = defineProps(["name", "urlImgPoke"])

</script>

<template>
    <div class="col-4">
        <div class="card p-2 mb-3 cardListPokemon">
            <p class="text-center">{{ pokemon.name }}</p>
            <img 
            :src="urlImgPoke" class="card-img-top" 
            alt="..."
            height="90"
            >
        </div>
    </div>
</template>

<style scoped>
    .cardListPokemon{
        background: rgb(72,63,251);
        background: radial-gradient(circle, rgba(94, 87, 232, 0.6) 0%, rgba(70,252,244,0.2) 100%);
        cursor: pointer;
    }
    .cardListPokemon:hover{
        background: rgb(72,63,251);
        background: radial-gradient(circle, rgba(72,63,251,0.6) 0%, rgba(70,252,244,0.4) 100%);
    }
    .cardListPokemon img:hover{
        padding: .2rem;
    }
</style>