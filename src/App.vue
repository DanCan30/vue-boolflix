<template>
  <div id="app">

    <Header @search="getShowsList" />
    <Main 
    
      :films="filmsList"
      :TVSeries="TVSeriesList"/>

  </div>
</template>

<script>
import axios from "axios";
import Header from './components/Header.vue';
import Main from './components/Main.vue';

export default {
  name: 'App',

  data: function() {
    return {
      apiFilmsUrl: "https://api.themoviedb.org/3/search/movie?api_key=1003857666890380902d6e3595e8622a&language=it-IT&query=",
      apiTVSeriesUrl: "https://api.themoviedb.org/3/search/tv?api_key=1003857666890380902d6e3595e8622a&language=it-IT&query=",
      filmsList: [],
      TVSeriesList: [],
    }
  },

  components: {
    Header,
    Main,
  },
  methods: {

    getShowsList: function(search) {

      axios.get(`${this.apiFilmsUrl}${search}`)
      .then((result) => {
        this.filmsList = result.data.results;
        }
      );

      axios.get(`${this.apiTVSeriesUrl}${search}`)
      .then((result) => {
        this.TVSeriesList = result.data.results;
        }
      );
    }
  },

  created() {
    this.getShowsList();
  }
}
</script>

<style lang="scss">
@import "~@fortawesome/fontawesome-free/css/all.css";
@import "./assets/styles/common.scss";

  div#app {
    font-family: 'Poppins', sans-serif;
  }

</style>
