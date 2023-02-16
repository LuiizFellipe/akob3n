
<template>
  <main class="main" style="padding: 20px;">
    <div class="banner-home">
      <b-carousel
        id="carousel-fade"
        style="text-shadow: 0px 0px 2px #000"
        fade
        :interval="4000"
        img-width="1024"
        img-height="480"
      >
        <b-carousel-slide
          v-for="x in banners"  :key="x.id"
          v-bind:img-src="x.large_cover_image"
          caption=""
        >
          <div class="columns-banner">
            <div></div> 
            <div>
              <p class="ano-filme">{{x.year}}</p>
              <h2>{{x.title}}</h2>
              <p class="sinopse-filme">{{x.synopsis}}</p>
               <router-link :to="{name: 'movie', params:{id: x.id}}"><b-icon icon="play-circle-fill"></b-icon> Assistir</router-link>
            </div>
          </div>
        </b-carousel-slide>
      </b-carousel>
    </div>
    <h3 style="text-align: left;color: white;padding-left: 20px;">Recentes</h3>
    <div class="filmes">
      <div v-for="x in banners"  :key="x.id">
        <img class="img-filmes" :src="x.large_cover_image" alt="">
        <p>{{x.title_long}}</p>
       <router-link :to="{name: 'movie', params:{id: x.id}}">Ver filme</router-link>
      </div>
    </div>
    <section style="padding-top: 50px; overflow: hidden; min-height: 400px">
      <h3 style="text-align: left;color: white;padding-left: 20px;">Mais famosos</h3>
      <div class="famoso">
      <div class="item-filme" v-for="y in download"  :key="y.id">
        <div class="overlay-img"></div>
        <img class="img-filmes" :src="y.large_cover_image" alt="">
        <p class="titulo-filme">{{y.title_long}}</p>
        <div class="filme-descricao">
        <div class="col-1">
          <img class="img-filmes" :src="y.large_cover_image" alt="">
          <p>Avaliação: {{y.rating}}</p>
          <router-link :to="{name: 'movie', params:{id: y.id}}"><b-icon icon="play-circle-fill"></b-icon> Assistir</router-link>
        </div>
        <div class="col-2">
          <div class="gen">
            <p v-for="gen in y.genres" :key="gen.id">{{gen}}</p>
          </div>
          <p class="sinopse">{{y.synopsis}}</p>
        </div>
        </div>
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

/* width */
::-webkit-scrollbar {
  width: 5px;
}
/* Track */
::-webkit-scrollbar-track {
  background: #f1f1f1;
}
/* Handle */
::-webkit-scrollbar-thumb {
  background: #888; 
  border-radius: 4px;
}
/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: #555; 
}
.banner-home {
  border-radius: 20px;
  overflow: hidden;
  margin-bottom: 65px;
}
.banner-home .carousel-caption {
  height: 100%;
  right: 0%;
  bottom: 0;
  padding-top: 0;
  padding-bottom: 0;
  left: 0;
}
.banner-home .carousel-item .columns-banner {
  display: flex;
  flex-direction: row;
  height: 100%;
}
.banner-home .carousel-item .columns-banner div:nth-child(1) {
  width: 30%;
  position: relative;
  max-width: 260px;
}
.banner-home .carousel-item .columns-banner div:nth-child(1):after {
  content: "";
  position: absolute;
  left: 0;
  width: 105%;
  min-width: 260px;
  height: 100%;
  background: rgb(26,26,26);
  background: linear-gradient(90deg, rgba(26,26,26,0) 0%, rgba(26,26,26,0.9420289855072463) 85%, rgba(26,26,26,1) 100%);
  z-index: 2;
}
.banner-home .carousel-item .columns-banner div:nth-child(2) {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: baseline;
  width: 70%;
  z-index: 3;
  padding: 0 5% 0 10%;
  text-align: initial;
}
.banner-home .carousel-item .columns-banner div:nth-child(2) .sinopse-filme {
  font-size: 12px;
  max-width: 400px;
  border-radius: 20px;
  overflow: hidden;
}
.banner-home .carousel-item .columns-banner div:nth-child(2) .ano-filme {
  background-color: #F4515F;
  width: max-content;
  padding: 3px 10px;
  border-radius: 50px;
  font-size: 12px;
}
.banner-home .carousel-item .columns-banner div:nth-child(2) h2 {
  font-size: 38px;
  font-weight: 800;
  margin-top: 10px;
}
.banner-home .carousel-item .columns-banner div:nth-child(2) a {
  color: #fff;
  background-color: #6C43BF;
  padding: 8px 20px;
  border-radius: 50px;
  text-decoration: none;
  margin-top: 20px;
}
.banner-home .carousel-item img {
  max-height: 390px;
  object-fit: cover;
  max-width: 260px;
}
.famoso{
  flex-direction: row;
  display: flex;
}
.famoso img{
  width:100px;
  height: 100px;
}
.famoso p{
  font-size: 12px;
  color: #fff;
  margin-top:10px;
  font-weight: bold;
  position: relative;
  z-index: 2;
}
.item-filme {
  display: flex;
  align-items: bottom;
  position: relative;
  border-radius: 15px;
  padding: 10px;
  margin-left:5px;
  text-align: left;
  width: 400px;
  margin-right: 5px;
  min-height: 250px;
  cursor: pointer;
  align-items: flex-end;
  transition: all 0.3s ease-in-out;
}
.item-filme .titulo-filme {
  font-size: 18px;
}
.item-filme .img-filmes {
  position: absolute;
  top: 0px;
  left: 0px;
  border-radius: 15px;
  width: 100%;
  height: 100%;
  margin: 0px;
  padding: 0px;
}
.item-filme .overlay-img {
  position: absolute;
  top: 0px;
  left: 0px;
  border-radius: 15px;
  width: 100%;
  height: 100%;
  margin: 0px;
  padding: 0px;
  z-index: 2;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.4), rgba(0, 0, 0, 0.9));
}
.item-filme .filme-descricao {
  display: flex;
  flex-direction: row;
  position: absolute;
  top: 0px;
  left: 0px;
  overflow: auto;
  background-color: #1A1A1A;
  border-radius: 10px;
  transform: translateX(100vw);
  width: 420px;
  height: 300px;
  z-index: 5;
  opacity: 0;
  box-shadow: 5px 5px 10px rgb(0, 0, 0, 0.5);
  transition: all 0s ease-in-out;
  transition: opacity 0.3s ease-in-out;
}
.item-filme:nth-last-child(1) .filme-descricao,
.item-filme:nth-last-child(2) .filme-descricao{
  top: 0px;
  right: 0px;
  left: auto;
}
.item-filme .filme-descricao .col-1 {
  width: 30%;
  padding: 10px;
}
.item-filme .filme-descricao .col-2 {
  width: 70%;
  padding: 10px;
}
.item-filme .filme-descricao .col-1 .img-filmes {
  position: relative;
  width: 100%;
  height: auto;
  border-radius: 8px;
}
.item-filme .filme-descricao .col-1 a {
  width: 100%;
  color: #fff;
  background-color: #6C43BF;
  padding: 8px 12px;
  border-radius: 50px;
  text-decoration: none;
}
.item-filme .filme-descricao .col-2 .gen p {
  display: inline;
  text-transform: uppercase;
  font-size: 10px;
  margin-top: -5px
}
.item-filme .filme-descricao .col-2 .gen p:after {
  content: ",";
  margin-right: 5px;
}
.item-filme .filme-descricao .col-2 .gen p:nth-last-child(1):after {
  content: "";
}
.item-filme .filme-descricao .col-2 .sinopse {
  overflow: auto;
}
.item-filme:hover .filme-descricao {
  opacity: 1;
  transform: translateX(0%);
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
  width: 100%;
  object-fit: cover;
  height: 300px;
  margin-left:5px;
  margin-right: 5px;
}
</style>