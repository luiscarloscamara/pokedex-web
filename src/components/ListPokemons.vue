<script setup>
const pokemon = defineProps(["name"])

</script>

<template>
    <div class="col-4">
        <div class="card mb-3">
            <p class="text-center">{{ pokemon.name }}</p>
        </div>
    </div>
</template>

<style>

</style>