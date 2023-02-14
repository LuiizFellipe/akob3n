
<template>
  <main class="main" style="padding: 20px;">
    <h3 style="text-align: left;color: white;padding-left: 20px;">Recentes</h3>
    <div class="filmes">
      <div v-for="x in banners"  :key="x.id">
        <img class="img-filmes" :src="x.large_cover_image" alt="">
        <p>{{x.title_long}}</p>
       <router-link :to="{name: 'movie', params:{id: x.id}}">Ver filme</router-link>
      </div>
    </div>
    <section style="background-color: #1d1d1d;padding-top: 50px;">
      <h3 style="text-align: left;color: white;padding-left: 20px;">Mais famosos</h3>
      <div class="famoso">
      <div v-for="y in download"  :key="y.id">
        <img class="img-filmes" :src="y.large_cover_image" alt="">
        <p>{{y.title_long}}</p>
        <p>{{y.genres}}</p>
      </div>
    </div>
    </section>
  </main>

</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      banners: [],
      download: [],
    };
  },
  methods: {
    async getData() {
      axios.get('https://yts.mx/api/v2/list_movies.json?limit=5&sort_by=date_added&order_by=desc')
        .then(response => {
          console.log(response.data.data.movies[0]);
          this.banners = response.data.data.movies;
        })
        .catch(error => {
          console.log(error)
        })
    },
    getData2() {
      axios.get('https://yts.mx/api/v2/list_movies.json?limit=5&sort_by=download_count&order_by=desc')
        .then(count => {
          console.log(count.data.data.movies[0]);
          this.download = count.data.data.movies;
        })
        .catch(error => {
          console.log(error)
        })
    },
  },
  created() {
    this.getData();
    this.getData2();
  }
}
</script>

<style>
.famoso{
  flex-direction: row;
  display: flex;
}
.famoso img{
  width:100px;
  height: 100px;
}
.famoso div{
  background-color: #eaeaea;
  border-radius: 20px;
  padding: 10px;
  margin-left:5px;
  text-align: left;
  width: 400px;
  margin-right: 5px;
}
.famoso p{
  font-size: 12px;
  color: black;
  margin-top:10px;
  font-weight: bold;
}
.filmes {
  display: flex;
  position: relative;
  padding: 10px;
  flex-direction: row;
  width: 100%;
}
.filmes p{
  font-size: 12px;
  color: white;
  margin-top:10px;
  font-weight: bold;
}
.filmes button{
    background-color: red;
    border:0;
    color:white;
    transition: 1s;
    padding:10px 20px 10px 20px;
    border-radius: 20px;
}
.filmes button:hover{
  background-color: white;
  color:red;
  transition: 1s;
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
