<template>
  <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/loginUsuario">login</router-link>
  </nav>
  <router-view />
</template>

<script>
// import LoginUsuario from '@/views/LoginUsuario.vue'; // Importe o componente de login

export default {
  components: {
    // LoginUsuario // Adicione o componente de login como um componente filho
  }
};
</script>

<style>
* {
  font-family: Montserrat, Avenir, Helvetica, Arial, sans-serif;
}

body {
  margin: 0;
}

a {
  text-decoration: none;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width: 100vw;
  height: 100vh;
  margin: 0;
  display: flex;
  flex-direction: column;
  align-items: stretch;
  align-content: center;
  background-color: white;
  /*background-image: linear-gradient(rgb(255, 255, 255), rgb(226, 219, 219));*/
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
}
</style>
