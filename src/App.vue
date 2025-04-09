<script setup>
import {onMounted, reactive} from "vue";

let pokemons = reactive();

onMounted(()=>{
  fetch("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
  .then(res => res.json())
  // .then(res => console.log(res));
  .then(res => pokemons = res.results);
})

</script>

<template>
  <!-- NAVBAR -->
  <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
    <div class="container-fluid">
      <router-link class="navbar-brand" to="/">
        <img src="./assets/logo.png" alt="Logo" width="30" height="30" class="d-inline-block align-text-top">
        Pokedéx
      </router-link>
      <!-- <router-link class="navbar-brand" to="/">Pokedex</router-link> -->
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav">
          <li class="nav-item">
            <router-link class="nav-link" aria-current="page" to="/">Home</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/about">About</router-link>
          </li>
          <!-- <li class="nav-item">
            <a class="nav-link" href="#">Pricing</a>
          </li>
          <li class="nav-item">
            <a class="nav-link disabled" aria-disabled="true">Disabled</a>
          </li> -->
        </ul>
      </div>
    </div>
</nav>

<!-- CONTENT -->
 <div class="main">
    <router-view></router-view>
 </div>

 <footer class="bg-primary text-light">
    <div>
      Luís Pokedex em {{ new Date().getFullYear() }}
    </div>
 </footer>
</template>

<style>
  footer{
    position: fixed;
    bottom: 0;
    width: 100%;
    display: flex;
    align-items:center;
    justify-content: center;
    height: 56px; 
  }
</style>