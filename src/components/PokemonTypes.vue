<script setup>
const props = defineProps({
  types: {
    type: Array,
    required: true
  }
})

const typeColors = {
  normal: "#A8A77A",
  fire: "#EE8130",
  water: "#6390F0",
  electric: "#F7D02C",
  grass: "#7AC74C",
  ice: "#96D9D6",
  fighting: "#C22E28",
  poison: "#A33EA1",
  ground: "#E2BF65",
  flying: "#A98FF3",
  psychic: "#F95587",
  bug: "#A6B91A",
  rock: "#B6A136",
  ghost: "#735797",
  dragon: "#6F35FC",
  dark: "#705746",
  steel: "#B7B7CE",
  fairy: "#D685AD"
}
</script>

<template>
  <div class="type-container">
    <span
      v-for="type in types"
      :key="type"
      class="type-badge"
      :style="{ backgroundColor: typeColors[type] || '#ccc' }"
    >
      {{ type }}
    </span>
  </div>
</template>

<style scoped>
.type-container {
  display: flex;
  gap: 0.5rem;
  justify-content: center;
  flex-wrap: wrap;
}

.type-badge {
  padding: 0.25rem 0.5rem;
  border-radius: 12px;
  color: white;
  font-weight: bold;
  text-transform: capitalize;
}
</style>
