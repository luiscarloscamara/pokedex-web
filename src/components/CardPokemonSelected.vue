<script setup>
import { ref, watch } from 'vue'
import PokemonTypes from './PokemonTypes.vue'

const pokemon = defineProps(["name", "id", "types", "species", "img", "loading"]);
const emit = defineEmits(["openModal"]);

const ovoImg = new URL("../assets/ovo.png", import.meta.url).href;
const infoImg = new URL("../assets/button.png", import.meta.url).href;

const imgSrc = ref(pokemon.img);

watch(() => pokemon.img, (newImg) => {
  imgSrc.value = newImg
})
</script>

<template>
  <div
    class="card CardPokemonSelected position-relative"
    :class="loading ? '' : 'animate__animated animate__flipInY'"
  >
    <!-- Botão de abrir modal -->
    <button class="info-button" @click="emit('openModal')">
      <img :src="infoImg" alt="Info" />
    </button>

    <!-- Imagem -->
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

    <!-- Dados -->
    <div class="card-body text-center">
        <h5 class="card-title name">{{ pokemon.name || '???' }}</h5>

        <hr class="custom-hr">

        <div class="none-element row justify-content-between text-start px-3 d-none d-md-block">
            <section>
                <strong>ID: </strong>
                <span>#{{ pokemon.id }}</span>
            </section>
            <section>
                <strong>Espécie: </strong>
                <span>{{ pokemon.species }}</span>
            </section>
        </div>

        <div class="none-element row mt-3 d-none d-md-block">
            <section class="col text-center">
                <PokemonTypes :types="pokemon.types" />
            </section>
        </div>
    </div>




  </div>
</template>

<style scoped>
.CardPokemonSelected {
  height: 75vh;
  background: radial-gradient(circle, rgba(117, 228, 130, 0.6) 0%, rgba(188, 161, 39, 0.2) 100%);
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  padding: 1rem;
  position: relative;
}

.card-img-top {
  max-height: 300px;
  width: auto;
  object-fit: contain;
  margin-bottom: 1rem;
}

.name {
  font-size: 1.5rem;
  font-weight: bold;
  text-transform: capitalize;
}

.custom-hr {
  border: none;
  height: 3px;
  background: linear-gradient(to right, #e0491f, #f0ad4e, #5cb85c);
  margin: 0.5rem auto 1rem auto;
  width: 80%;
  border-radius: 5px;
}

.info-button {
  position: absolute;
  top: 0.5rem;
  right: 0.5rem;
  background: transparent;
  border: none;
  padding: 0;
  cursor: pointer;
}

.info-button img {
  width: 24px;
  height: 24px;
}

@media (max-width: 768px) {
  .CardPokemonSelected {
    height: 30vh;
    width: 40%;
    margin: 0 auto 10px auto;
  }

  .CardPokemonSelected img {
    height: 100px;
  }
}

.info-button img {
    width: 18px;
    height: 18px;
  }
</style>