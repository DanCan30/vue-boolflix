<template>
  <div id="app">

    <Header @searchInput="getShowsList" @isResearchDone="checkResearch" />
    <Main 
    
      :films="filmsList"
      :TVSeries="TVSeriesList"
      :popular="mostPopularShows"
      :researchDone="researchDone"/>

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
      apiKey: "1003857666890380902d6e3595e8622a",

      currentLanguage: "it-IT",
      
      apiFilmsUrl: "https://api.themoviedb.org/3/search/movie",
      
      apiTVSeriesUrl: "https://api.themoviedb.org/3/search/tv",
      
      apiPopularSeriesUrl: "https://api.themoviedb.org/3/tv/popular",

      apiFilmsGenresUrl: "https://api.themoviedb.org/3/genre/movie/list",

      apiTVSeriesGenresUrl: "https://api.themoviedb.org/3/genre/tv/list",
      
      filmsList: [],
      
      TVSeriesList: [],

      mostPopularShows: [],

      uniqueGenres: [],
      
      researchDone: false,
    }
  },

  components: {
    Header,
    Main,
  },
  methods: {

    getShowsList: function(searchResult) {

      axios.get(`${this.apiFilmsUrl}?api_key=${this.apiKey}&language=${this.currentLanguage}&query=${searchResult}`)

      .then((result) => {
          this.filmsList = result.data.results;

        }
      );

      axios.get(`${this.apiTVSeriesUrl}?api_key=${this.apiKey}&language=${this.currentLanguage}&query=${searchResult}`)

      .then((result) => {
        this.TVSeriesList = result.data.results;
        }
      );

      axios.get(`${this.apiPopularSeriesUrl}?api_key=${this.apiKey}&language=${this.currentLanguage}`)

      .then((result) => {
        this.mostPopularShows = result.data.results;
        console.log(result.data.results)
        }
      );

    },

    getUniqueGenres: function() {
      axios.get(`${this.apiFilmsGenresUrl}?api_key=${this.apiKey}&language=${this.currentLanguage}`)
      .then((result) => {

        this.uniqueGenres = result.data.genres;
        console.log(this.uniqueGenres);
        
      });

    },

    checkResearch: function(value) {
      this.researchDone = value;
    }
  },

  created() {
    this.getShowsList();
    this.getUniqueGenres();
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
