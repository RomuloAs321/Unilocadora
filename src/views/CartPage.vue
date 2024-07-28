<template>
  <div>
    <h1>Carrinho</h1>
    <ul>
      <li v-for="movie in user.cart" :key="movie.id">
        <MovieBoxCart :movie="movie" @remove-from-cart="removeFromCart"></MovieBoxCart>
      </li>
    </ul>
    <button @click="closeCart">Fechar Carrinho</button>
  </div>
</template>

<script>
import MovieBoxCart from '@/components/MovieBoxCart.vue';
import axios from 'axios';

export default {
  components: {
    MovieBoxCart,
  },
  data() {
    return {
      user: {}, // Atualiza o data para receber os dados do usuário
    };
  },
  created() {
    this.fetchUser();
  },
  methods: {
    fetchUser() {
      const userId = localStorage.getItem('userId');
      if (userId) {
        axios.get(`http://localhost:3000/users/${userId}`)
          .then(response => {
            this.user = response.data;
          })
          .catch(error => {
            console.error('Erro ao recuperar os dados do usuário:', error);
          });
      } else {
        console.error('Usuário não autenticado');
      }
    },
    removeFromCart(movie) {
  const index = this.user.cart.findIndex(item => item.id === movie.id);
  if (index !== -1) {
    this.user.cart.splice(index, 1); // Remove o filme do carrinho pelo índice
    this.updateUser(); // Atualiza o usuário no servidor para refletir as alterações no carrinho
  }
},
    closeCart() {
      // Move os filmes do carrinho para a prateleira antes de esvaziar o carrinho
      this.user.shelf.push(...this.user.cart);
      this.user.cart = []; // Esvazia o carrinho
      this.updateUser(); // Atualiza o usuário no servidor para refletir as alterações no carrinho e na prateleira
    },
    updateUser() {
      const userId = localStorage.getItem('userId');
      axios.put(`http://localhost:3000/users/${userId}`, this.user)
        .then(response => {
          console.log('Dados do usuário atualizados:', response.data);
        })
        .catch(error => {
          console.error('Erro ao atualizar os dados do usuário:', error);
        });
    },
  },
};
</script>

<style scoped>
.movie-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
}

.movie-grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 20px;
}

.col-5 {
  flex-basis: 20%;
}

.movie-box {
  width: 250px;
  height: 400px;
  padding: 20px;
  box-sizing: border-box;
  background-color: red;
  border: 2px solid #ff0000;
  border-radius: 5px;
  color: white;
  font-family: "Comic Sans MS", cursive;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
}

.movie-box img {
  width: 100%;
  max-height: 300px;
  object-fit: cover;
  margin-bottom: 10px;
}

.movie-box p {
  margin: 0;
}


button {
  display: inline-block;
  padding: 10px 10px;
  background-color: #f4f7f8;
  color: rgb(5, 5, 5);
  border-radius: 5px;
  text-decoration: none;
  margin-right: 10px;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #2980b9;
  color: white;
}
</style>
