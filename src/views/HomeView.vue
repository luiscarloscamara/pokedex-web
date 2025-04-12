<script setup>
  import { onMounted, reactive, ref, computed } from "vue";
  import ListPokemons from "@/components/ListPokemons.vue";
  import CardPokemonSelected from "@/components/CardPokemonSelected.vue";
  import PokemonData from "@/components/PokemonData.vue";


  let urlImgPoke = ref("https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/");
  let pokemons = reactive(ref());
  let searchPokemonField = ref("");
  const pokemonSelected = ref(null);
  let loading = ref(false);
  const ovoImg = new URL("@/assets/ovo.png", import.meta.url).href;
  const isModalOpen = ref(false);

  onMounted(() => {
    fetch("https://pokeapi.co/api/v2/pokemon?limit=10000&offset=0")
      .then(res => res.json())
      .then(res => {
        pokemons.value = res.results;
      });
  });

  const PokemonsFiltered = computed(() => {
    if (pokemons.value && searchPokemonField.value) {
      return pokemons.value.filter(pokemon =>
        pokemon.name.toLowerCase().includes(searchPokemonField.value.toLowerCase())
      );
    }
    return pokemons.value;
  });

  const formatPokemonDetails = (data, img) => {
    return {
      name: data.name,
      id: data.id,
      types: data.types.map(t => t.type.name),
      species: data.species.name,
      img,
      height: data.height,
      base_experience: data.base_experience,
      sprites: Object.entries(data.sprites)
        .filter(([key, value]) => typeof value === 'string' && value)
        .reduce((acc, [key, value]) => ({ ...acc, [key]: value }), {}),
      moves: data.moves,
      game_indices: data.game_indices.map(g => ({ version: { name: g.version.name } })),
      // game_indices: details.data.game_indices,
      speciesUrl: data.species.url
    };
  };

  const selectPokemon = async (pokemon) => {
    loading.value = true;
    try {
      const res = await fetch(pokemon.url);
      const data = await res.json();
      const img = await getImgUrl(data.id);

      pokemonSelected.value = formatPokemonDetails(data, img);
    } catch (err) {
      alert(err);
    } finally {
      loading.value = false;
    }
  };

  const getImgUrl = async (id) => {
    const url = urlImgPoke.value + id + '.png';
    try {
      const res = await fetch(url);
      if (!res.ok) throw new Error("Imagem não encontrada");
      return url;
    } catch {
      return ovoImg;
    }
  };
</script>

<template>
  <main>
    <div class="container">
      <div class="row mt-4">
        <div class="col-sm-12 col-md-6">
          <CardPokemonSelected 
            :name="pokemonSelected?.name"
            :id="pokemonSelected?.id"
            :types="pokemonSelected?.types"
            :species="pokemonSelected?.species"
            :img="pokemonSelected?.img"
            :loading="loading"
            @openModal="isModalOpen = true"
          />
        </div>

        <div class="col-sm-12 col-md-6">
          <div class="card card-list">
            <div class="card-body row">
              <div class="sticky-top bg-white pb-2">
                <div class="mb-3">
                  <label hidden for="searchPokemonField" class="form-label">Pesquisar...</label>
                  <input 
                    v-model="searchPokemonField"
                    type="text" 
                    class="form-control" 
                    id="searchPokemonField" 
                    placeholder="Pesquisar...">
                </div>
              </div>

              <ListPokemons
                v-for="pokemon in PokemonsFiltered"
                :key="pokemon.name"
                :name="pokemon.name"
                :urlImgPoke="urlImgPoke + pokemon.url.split('/')[6] + '.png'"
                @click="selectPokemon(pokemon)"
              />
            </div>
          </div>
        </div>
      </div>
    </div>

    <PokemonData
      v-if="isModalOpen && pokemonSelected"
      :pokemon="pokemonSelected"
      @close="isModalOpen = false"
    />
  </main>
</template>

<style>
  .card-list {
    max-height: 75vh;
    overflow-y: scroll;
    overflow-x: hidden;
  }
</style>
