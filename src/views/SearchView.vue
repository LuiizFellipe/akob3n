
<template>
    <main style="padding: 20px;">
      <h2>Pesquisar</h2>
      <input v-model="inputValue" class="search-item">
      <h3>{{ inputValue }}</h3>
      <button @click="getSearch">
       Procurar
      </button>
      <div id="searchMovies">
        <div v-for="x in searchItens" :key="x.id">
        <img class="img-filmes" :src="x.large_cover_image" alt="">
        <p>{{x.title_long}}</p>
        <router-link :to="{name: 'movie', params:{id: x.id}}">Ver filme</router-link>
      </div>
      </div>

    </main>
  
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        inputValue: '',
        searchItens: [],
        generos:this.downloadgenres
      };
    },
    methods: {
      async getSearch() {
        axios.post('https://yts.mx/api/v2/list_movies.json?query_term='+this.inputValue)
          .then(response => {
            console.log(response);
            this.searchItens = response.data.data.movies;
          })
          .catch(error => {
            console.log(error)
            this.inputValue = "Nao foi possivel encontrar"
          })
      },
    },
  }
  </script>
  
  <style>
  main{
    color:white;
  }
  #searchMovies{
    flex-direction: row;
    display: flex;
  }
  .famoso img{
    width:100px;
    height: 100px;
  }
  .img-filmes{
    border-radius: 20px;
    width: 95%;
    object-fit: cover;
    height: 300px;
    margin-left:5px;
    margin-right: 5px;
  }
  </style>
  