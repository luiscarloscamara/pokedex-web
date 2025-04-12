<script setup>
import {onMounted, reactive} from "vue";

let pokemons = reactive();

</script>

<template>
  <!-- NAVBAR -->
  <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
    <div class="container-fluid">
      <router-link class="navbar-brand" to="/">
        <img src="./assets/logo.png" alt="Logo" width="90" height="25" class="d-inline-block align-text-top">
      </router-link>
      <!-- <router-link class="navbar-brand" to="/">Pokedex</router-link> -->
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav">
          <li class="nav-item">
            <router-link class="nav-link" aria-current="page" to="/">Pokémons</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/about">Sobre</router-link>
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
  body{
    height: 100vh;
  }
  footer{
    position: fixed;
    bottom: 0;
    width: 100%;
    display: flex;
    align-items:center;
    justify-content: center;
    height: 56px;
    z-index: 1;
  }
</style>