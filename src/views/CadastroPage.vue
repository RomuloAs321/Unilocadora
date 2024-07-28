<template>
  <div>
    <h2>Cadastro</h2>
    <form @submit.prevent="register">
      <div>
        <label for="email">Email:</label>
        <input type="email" id="email" v-model="email" required>
      </div>
      <div>
        <label for="password">Senha:</label>
        <input type="password" id="password" v-model="password" required>
      </div>
      <button type="submit">Cadastrar</button>
    </form>
  </div>
</template>

<script>
import axios from '@/components/axios';

export default {
  data() {
    return {
      name: '',
      email: '',
      password: ''
    };
  },
  methods: {
    register() {
      const newUser = {
        email: this.email,
        password: this.password,
        cart: [],
        shelf: []
      };

      axios.post('/users', newUser)
        .then(response => {
          const user = response.data;
          this.$store.dispatch('updateUser', { user }); // Atualizar a sessão do usuário no Vuex

          // Redirecionar para a página do menu após o cadastro
          this.$router.push('/menu');
        })
        .catch(error => {
          console.error(error);
        });
    }
  }
}
</script>

<style scoped>
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

</style>