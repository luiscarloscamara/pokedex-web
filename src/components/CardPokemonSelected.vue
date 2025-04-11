<script setup>
const pokemon = defineProps (["name", "xp", "altura", "img", "loading"])
</script>

<template>
    <div 
    class="card CardPokemonSelected"
    :class="loading ? '' : 'animate__animated animate__flipInY'"
    >
        <img 
        :src="pokemon.img" 
        height="250"
        class="card-img-top pt-2" 
        alt="pokemon.name"
        >
        
        <div class="card-body">
          <h5 class="card-title text-center">{{ pokemon.name }}</h5>
          <hr>
          <div class="row">
            <section class="col">
                <strong>XP:</strong>
                <span>{{ pokemon.xp }}</span>
            </section>
            <section class="col">
                <strong>Altura:</strong>
                <span>{{ pokemon.altura }}</span>
            </section>

          </div>
          <!-- <a href="#" class="btn btn-primary">Go somewhere</a> -->
        </div>
    </div>
</template>

<style scoped>
   .CardPokemonSelected{
        height: 450px;
        background: rgb(72,63,251);
        background: radial-gradient(circle, rgba(117, 228, 130, 0.6) 0%, rgba(188, 161, 39, 0.2) 100%);
   } 
</style>