<template>
  <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/Menu">Menu</router-link> |
    <router-link to="/Cart">Carrinho </router-link> |
    <router-link to="/Shelf">Estante </router-link> |
  </nav>
  <router-view/>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

body {
      background-color: #16e785; 
    }

nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
