<template>
  <div>
    <h1>Estante</h1>
    <div class="movie-container movie-grid">
      <MovieBoxShelf
        v-for="movie in user.shelf"
        :key="movie.id"
        :movie="movie"
        @remove-from-shelf="removeFromShelf"
      ></MovieBoxShelf>
    </div>
  </div>
</template>

<script>
import MovieBoxShelf from '@/components/MovieBoxShelf.vue';
import axios from 'axios';

export default {
  components: {
    MovieBoxShelf,
  },
  data() {
    return {
      user: {},
    };
  },
  created() {
    this.fetchUser();
  },
  methods: {
    fetchUser() {
      const userId = localStorage.getItem('userId');
      if (userId) {
        axios
          .get(`http://localhost:3000/users/${userId}`)
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
    removeFromShelf(movie) {
      const index = this.user.shelf.findIndex(item => item.id === movie.id);
      if (index !== -1) {
        this.user.shelf.splice(index, 1); // Remove o filme da estante pelo índice
        this.updateUser(); // Atualiza o usuário no servidor para refletir as alterações na estante
      }
    },
    updateUser() {
      const userId = localStorage.getItem('userId');
      axios
        .put(`http://localhost:3000/users/${userId}`, this.user)
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
</style>
