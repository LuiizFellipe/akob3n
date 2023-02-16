<template>
  <div id="app">
    <nav>
      <h3 style="font-weight: 200;">AKOBEN</h3>
      <form @submit.prevent="search">
        <input  v-model="query" style="width: 100%;" placeholder="search movie">
        <button type="submit">Search</button>
      </form>
      <div>
        <div class="activate" v-if="$route.name === 'home'">
          <b-icon-house-fill></b-icon-house-fill><router-link to="/"> Home</router-link>
        </div>
        <div class="desativate" v-else>
          <b-icon-house></b-icon-house><router-link to="/"> Home</router-link>
        </div>
      </div>
      <b-icon-person class="icon-login"></b-icon-person>
    </nav>
    <router-view />
  </div>
</template>

<script>
export default{
  data(){
    return{
      query: '',
    }
  },
  methods:{
    search(){
      this.$router.push({ name: 'search', params: { query: this.query } })
    }
  }
}
</script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;400;500;600;700&display=swap');

#app {
  font-family: 'Space Grotesk', sans-serif;
  text-align: center;
  color: white;
  display: flex;
  flex-direction: row;
}

nav {
  padding: 30px;
  width: 15%;
}

nav img {
  width: 100px;
}

.activate {
  color: red !important;
}

.desativate {
  color: blue !important;
}

nav a {
  text-transform: uppercase;
  padding-top: 10px;
  padding-bottom: 10px;
  color: white;
  font-weight: bold;
  text-decoration: none !important;
}

.main {
  width: 85%;
}

#app {
  background-color: #1a1a1a;
}
</style>
