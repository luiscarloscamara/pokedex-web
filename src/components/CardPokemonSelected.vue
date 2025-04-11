<script setup>
import { ref, watch } from 'vue'

const pokemon = defineProps(["name", "xp", "altura", "img", "loading"])
const ovoImg = new URL("../assets/ovo.png", import.meta.url).href;
const imgSrc = ref(pokemon.img)

watch(() => pokemon.img, (newImg) => {
  imgSrc.value = newImg
})
</script>

<template>
    <div 
      class="card CardPokemonSelected"
      :class="loading ? '' : 'animate__animated animate__flipInY'"
    >
      <img
        v-if="pokemon.name"
        :src="imgSrc"
        @error="imgSrc = ovoImg"
        height="250"
        class="card-img-top mb-4" 
        :alt="pokemon.name"
      >
      <img
        v-else
        :src="ovoImg" 
        height="250"
        class="card-img-top mb-4" 
        alt="???"
      >
      
      <div class="card-body text-center">
        <h5 class="card-title">{{ pokemon.name || '???' }}</h5>
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
      </div>
    </div>
  </template>

<style scoped>
.card-body {
  width: 100%;
  text-align: center;
}

.CardPokemonSelected {
    height: 75vh;
    background: radial-gradient(circle, rgba(117, 228, 130, 0.6) 0%, rgba(188, 161, 39, 0.2) 100%);
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
    padding: 1rem;
}

.card-img-top {
    max-height: 300px;
    width: auto;
    object-fit: contain;
    margin-bottom: 1rem; /* Espaço entre imagem e infos */
}

@media (max-width: 768px) {
    .CardPokemonSelected{
        height: 30vh;
        width: 40%;
        margin: 0 auto 10px auto;
    }
    .CardPokemonSelected img{
        height: 100px;
    }
}
</style>