<script setup>
  import { onMounted, reactive, ref, computed } from "vue";
  import ListPokemons from "@/components/ListPokemons.vue";
  import CardPokemonSelected from "@/components/CardPokemonSelected.vue";

  // let urlImgPoke = ref("https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/")
  let urlImgPoke = ref("https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/");
  let pokemons = reactive(ref());
  let searchPokemonField = ref("")
  let pokemonSelected = reactive(ref());
  let loading= ref(false)
  const ovoImg = new URL("@/assets/ovo.png", import.meta.url).href;

  onMounted(() => {
    fetch("https://pokeapi.co/api/v2/pokemon?limit=10000&offset=0")
      .then(res => res.json())
      .then(res => {
        pokemons.value = res.results;
      });
  });

  const PokemonsFiltered = computed(()=>{
    if(pokemons.value && searchPokemonField.value){
      return pokemons.value.filter(pokemon=>
        pokemon.name.toLowerCase().includes(searchPokemonField.value.toLowerCase())
      )
    }
    return pokemons.value;
  })


  const selectPokemon = async (pokemon) => {
    loading.value = true;
    try {
      const res = await fetch(pokemon.url);
      const data = await res.json();

      const id = data.id;
      const img = await getImgUrl(id);
      const types = data.types.map(t => t.type.name);
      const species = data.species.name;

      pokemonSelected.value = {
        name: data.name,
        id,
        types,
        species,
        img,
      };
    } catch (err) {
      alert(err);
    } finally {
      loading.value = false;
    }
  };
  // const selectPokemon = (pokemon) => {
  //   loading.value = true;
  //   await fetch(pokemon.url)
  //   .then(res => res.json())
  //   .then(res => pokemonSelected.value = res);
  //   .catch(err => alert(err))
  //   .finally(()=>{
  //     loading.value = false;
  //   })
  //   console.log(pokemon)
  // }

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
          />

          <!-- <div class="card" style="width: 18rem;">
            <img src="https://www.pokemon.com/static-assets/content-assets/cms2/img/pokedex/full/149.png" class="card-img-top" alt="...">
            <div class="card-body">
              <h5 class="card-title">Card title</h5>
              <p class="card-text">Some quick example text to build on the card title and make up the bulk of the card's content.</p>
              <a href="#" class="btn btn-primary">Go somewhere</a>
            </div>
          </div> -->

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
  </main>
</template>

<style>
  .card-list {
    max-height: 75vh;
    overflow-y: scroll;
    overflow-x: hidden;
  }
</style>
