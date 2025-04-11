<script setup>
const pokemon = defineProps(["name", "urlImgPoke"])
const ovoImg = new URL("../assets/ovo.png", import.meta.url).href;
</script>

<template>
  <div class="col-4">
    <div class="card p-2 mb-3 cardListPokemon">
      <p class="text-center">{{ pokemon.name }}</p>
      <img 
        :src="pokemon.urlImgPoke"
        @error="event => event.target.src = ovoImg"
        class="card-img-top" 
        alt="Imagem do Pokémon"
        height="90"
      >
    </div>
  </div>
</template>


<style scoped>
.cardListPokemon {
  background: radial-gradient(circle, rgba(94, 87, 232, 0.6) 0%, rgba(70, 252, 244, 0.2) 100%);
  cursor: pointer;
  height: 160px; /* ou outro valor fixo confortável */
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
}

.cardListPokemon:hover {
  background: radial-gradient(circle, rgba(72, 63, 251, 0.6) 0%, rgba(70, 252, 244, 0.4) 100%);
}

.cardListPokemon img {
  object-fit: contain;
  height: 90px;
  width: 90px;
}

.cardListPokemon img:hover {
  padding: 0.2rem;
}
</style>