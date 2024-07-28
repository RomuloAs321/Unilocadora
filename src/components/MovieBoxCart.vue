<template>
    <div class="movie-box">
      <img :src="getImageUrl(movie.image)" :alt="movie.name" />
      <p>{{ movie.name }}</p>
      <button @click="removeFromCart">Remover</button>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      movie: {
        type: Object,
        required: true
      }
    },
    methods: {
      getImageUrl(image) {
        if (image.startsWith('@/assets/')) {
          return require(`@/${image.substring(2)}`);
        }
        return image;
      },
      removeFromCart() {
        this.$emit('remove-from-cart', this.movie);
      }
    }
  }
  </script>
  
  <style scoped>
  .movie-box {
    width: 100%;
    padding: 10px;
    box-sizing: border-box;
    background-color: red;
    border: 2px solid #ff0000;
    border-radius: 5px;
    color: white;
    font-family: "Comic Sans MS", cursive;
    margin-bottom: 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
  
  .movie-box img {
    width: 80%;
    max-height: 200px;
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
  