<template>
  <div>
    <h1>Menu</h1>
    <div class="movie-container movie-grid">
      <MovieBox
        v-for="movie in movies"
        :key="movie.id"
        :movie="movie"
        class="col-5"
        @add-to-cart="addToCart"
      ></MovieBox>
    </div>
    <button @click="goToCart">Carrinho</button>
  </div>
</template>

<script>
import MovieBox from '@/components/MovieBox.vue';
import axios from 'axios';

export default {
  components: {
    MovieBox,
  },
  data() {
    return {
      movies: [],
      user: null,
    };
  },
  created() {
    this.fetchMovies();
    this.fetchUser();
  },
  methods: {
    fetchMovies() {
      axios
        .get('http://localhost:3000/movies')
        .then(response => {
          this.movies = response.data;
        })
        .catch(error => {
          console.error('Erro ao recuperar a lista de filmes:', error);
        });
    },
    fetchUser() {
      const userId = localStorage.getItem('userId');
      axios
        .get(`http://localhost:3000/users/${userId}`)
        .then(response => {
          this.user = response.data;
        })
        .catch(error => {
          console.error('Erro ao recuperar os dados do usuário:', error);
        });
    },
    goToCart() {
      this.$router.push('/Cart');
    },
    addToCart(movie) {
      if (!this.user.cart) {
        this.user.cart = []; // Inicializa o carrinho do usuário se estiver vazio
      }
      this.user.cart.push(movie); // Adiciona o filme ao carrinho do usuário
      this.updateUser();
    },
    updateUser() {
      const userId = this.user.id;
      axios
        .put(`http://localhost:3000/users/${userId}`, this.user)
        .then(response => {
          console.log(response.data);
        })
        .catch(error => {
          console.error(error);
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

.button {
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

.button:hover {
  background-color: #2980b9;
  color: white; 
}
</style>
