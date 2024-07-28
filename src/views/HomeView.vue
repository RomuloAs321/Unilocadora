<template>
  <div class="home">
    <h1>Unilocadora</h1>
    <img alt="Vue logo" src="../assets/Fita.png">
    <div class="buttons">
      <router-link to="/login" class="button">Login</router-link> |
      <router-link to="/cadastro" class="button">Cadastro</router-link>
    </div>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'HomeView'
}
</script>

<style scoped>
.home {
  text-align: center;
}

.buttons {
  margin-top: 20px;
  text-align: center;
}

.button {
  display: inline-block;
  padding: 10px 10px;
  background-color: #f4f7f8;
  color: rgb(5, 5, 5);
  border-radius: 5px;
  text-decoration: none;
  margin-right: 10px;
}

.button:hover {
  background-color: #2980b9;
}

form {
  display: flex;
  flex-direction: column;
  align-items: center;
}

form div {
  display: flex;
  flex-direction: column;
  margin-bottom: 10px;
}

label {
  margin-bottom: 5px;
}

input {
  padding: 5px;
  border-radius: 5px;
  border: 1px solid #ccc;
}

img {
  max-width: 300px; 
}
</style>