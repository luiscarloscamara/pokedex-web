<script setup>
import { onMounted, ref, watch, computed } from 'vue'
import axios from 'axios'

const props = defineProps({
  pokemon: Object
})
const emit = defineEmits(["close"])

const img = ref(props.pokemon.img)
const ovoImg = new URL("../assets/ovo.png", import.meta.url).href;

const typeColors = {
  grass: '#78C850', fire: '#F08030', water: '#6890F0', bug: '#A8B820',
  normal: '#A8A878', poison: '#A040A0', electric: '#F8D030', ground: '#E0C068',
  fairy: '#EE99AC', fighting: '#C03028', psychic: '#F85888', rock: '#B8A038',
  ghost: '#705898', ice: '#98D8D8', dragon: '#7038F8', dark: '#705848',
  steel: '#B8B8D0', flying: '#A890F0'
}

const evolutions = ref([])
const isMovesExpanded = ref(false)  // Estado para controlar a expansão dos movimentos

const fetchEvolutions = async () => {
  if (!props.pokemon.speciesUrl) return
  const speciesRes = await axios.get(props.pokemon.speciesUrl)
  const evoChainUrl = speciesRes.data.evolution_chain.url
  const evoRes = await axios.get(evoChainUrl)

  const evoList = []
  let current = evoRes.data.chain
  do {
    evoList.push(current.species.name)
    current = current.evolves_to[0]
  } while (current && current.species)

  evolutions.value = evoList
}

watch(() => props.pokemon.speciesUrl, fetchEvolutions)
onMounted(fetchEvolutions)

const spriteUrls = computed(() => {
  const sprites = props.pokemon?.sprites || {};
  return Object.entries(sprites)
    .filter(([_, value]) => typeof value === 'string' && value !== null);
})

const gameNames = computed(() => {
  return props.pokemon.game_indices?.map(game => {
    return capitalize(game.version.name);
  }) || [];
})

const capitalize = (str) => {
  return str.charAt(0).toUpperCase() + str.slice(1);
}
</script>

<template>
  <div class="modal" @click.self="emit('close')">
    <div class="modal-content">
      <button class="close-btn" @click="emit('close')">X</button>

      <img
        :src="img"
        @error="img = ovoImg"
        alt="Imagem"
        class="pokemon-img"
      />
      <h2>{{ pokemon.name }}</h2>
      <p><strong>ID:</strong> #{{ pokemon.id }}</p>
      <p><strong>Espécie:</strong> {{ pokemon.species }}</p>

      <div class="attributes">
        <span
          v-for="type in pokemon.types"
          :key="type"
          class="type-pill"
          :style="{ backgroundColor: typeColors[type] || '#999' }"
        >
          {{ type }}
        </span>
        <span class="info-pill">Altura: {{ pokemon.height }}</span>
        <span class="info-pill">XP: {{ pokemon.base_experience }}</span>
      </div>

      <div class="sprites-section" v-if="spriteUrls.length">
        <h4>Sprites</h4>
        <div class="sprites">
          <img
            v-for="([key, url], index) in spriteUrls"
            :key="index"
            :src="url"
            :alt="key"
          />
        </div>
      </div>

      <div class="moves-section">
        <h4>Movimentos</h4>
        <div>
          <button class="expand-btn" @click="isMovesExpanded = !isMovesExpanded">
            <span v-if="isMovesExpanded">🔼</span>
            <span v-else>🔽</span>
            {{ isMovesExpanded ? 'Ver Menos' : 'Ver Todos' }}
          </button>
          <ul v-if="isMovesExpanded">
            <li v-for="move in pokemon.moves" :key="move.move.name">
              {{ move.move.name }}
            </li>
          </ul>
        </div>
      </div>

      <div class="games-section" v-if="gameNames.length">
        <h4>Games</h4>
        <div class="games-list">
          <span
            v-for="game in gameNames"
            :key="game"
            class="game-pill"
          >
            {{ game }}
          </span>
        </div>
      </div>

      <div class="evolutions-section" v-if="evolutions.length">
        <h4>Evoluções</h4>
        <ul>
          <li v-for="evo in evolutions" :key="evo">{{ evo }}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style scoped>
.modal {
  position: fixed;
  top: 0; left: 0;
  width: 100vw; height: 100vh;
  background: rgba(0, 0, 0, 0.6);
  z-index: 9999;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow-y: auto;
}

.modal-content {
  background: #fff;
  border-radius: 1rem;
  padding: 2rem;
  width: 90%;
  max-height: 90vh;
  overflow-y: auto;
  position: relative;
  z-index: 10000;
}

.close-btn {
  position: absolute;
  top: 1rem;
  right: 1rem;
  background: red;
  color: white;
  border: none;
  border-radius: 50%;
  width: 32px;
  height: 32px;
  font-weight: bold;
  cursor: pointer;
}

.pokemon-img {
  max-width: 200px;
  display: block;
  margin: 0 auto 1rem;
}

.type-pill, .info-pill, .game-pill {
  display: inline-block;
  padding: 0.3rem 0.7rem;
  border-radius: 1rem;
  margin: 0.3rem;
  color: white;
}

.attributes, .games-list {
  margin: 1rem 0;
}

.sprites img {
  height: 64px;
  margin: 0.3rem;
}

.expand-btn {
  background-color: #d5d5d5;
  color: white;
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 2rem;
  cursor: pointer;
  font-size: 16px;
  display: flex;
  align-items: center;
  transition: background-color 0.3s ease;
}

.expand-btn:hover {
  background-color: #d5d5d5;
}

.expand-btn span {
  margin-right: 0.5rem;
}

.expand-btn:focus {
  outline: none;
}
</style>
