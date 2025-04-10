<script setup>
const pokemon = defineProps(["name", "urlImgPoke"])

</script>

<template>
    <div class="col-4">
        <div class="card p-2 mb-3">
            <p class="text-center">{{ pokemon.name }}</p>
            <img 
            :src="urlImgPoke" class="card-img-top" 
            alt="..."
            height="80"
            >
        </div>
    </div>
</template>

<style>

</style>