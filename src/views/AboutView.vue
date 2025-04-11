<template>
  <div class="about d-flex align-items-center justify-content-center">
    <div class="background-image"></div>

    <div class="container move-up">
      <div class="card card-animated shadow-lg rounded-4 border-0 p-4 mx-auto bg-white bg-opacity-90">
        <div class="card-body">
          <h1 class="text-center mb-1 fw-bold text-primary">Sobre o Projeto</h1>
          
          <p class="fs-5 text-justify">
            Olá! Meu nome é <strong>Luís Carlos Santos Câmara</strong>, sou graduando em Ciência da Computação pela 
            <strong>Universidade Estadual de Santa Cruz (UESC)</strong>.
          </p>

          <p class="fs-5 text-justify">
            Desenvolvi esta <strong>Pokédex</strong> utilizando <strong>Vue.js</strong>, consumindo dados da 
            <a href="https://pokeapi.co" target="_blank">PokeAPI</a> para exibir informações de cada Pokémon disponível.
          </p>

          <hr class="my-4" />

          <h4 class="fw-semibold mb-3 text-center">Minhas Redes</h4>
          <div class="d-flex justify-content-center gap-4">
            <a 
              href="https://www.linkedin.com/in/luiscarloscamara/" 
              target="_blank" 
              class="btn btn-outline-primary btn-lg rounded-pill"
            >
              <i class="bi bi-linkedin me-2"></i>LinkedIn
            </a>
            <a 
              href="https://github.com/luiscarloscamara" 
              target="_blank" 
              class="btn btn-outline-dark btn-lg rounded-pill"
            >
              <i class="bi bi-github me-2"></i>GitHub
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.about {
  height: 90vh;
  width: 100%;
  position: relative;
  background: linear-gradient(135deg, #e0f7fa, #ffffff);
  overflow: hidden;
  padding: 0;
  margin: 0;
}

.background-image {
  position: absolute;
  top: 48%;
  left: 50%;
  width: 75vh;
  height: 90vh;
  background-image: url('https://archives.bulbagarden.net/media/upload/thumb/3/3e/Poké_Balls.png/640px-Poké_Balls.png');
  background-repeat: no-repeat;
  background-size: contain;
  opacity: 0.05;
  transform: translate(-50%, -50%);
  z-index: 0;
}

.card {
  max-width: 800px;
  width: 100%;
  position: relative;
  z-index: 1;
}

.move-up {
  transform: translateY(-50px);
}

.card-animated {
  animation: fadeInUp 1s ease-in-out;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

a {
  text-decoration: none;
}
</style>
