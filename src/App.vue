<template>
  <div id="app">

    <Header @searchInput="getShowsList" @isResearchDone="checkResearch" />
    <Main 
    
      :films="filmsList"
      :TVSeries="TVSeriesList"
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
      
      apiTVSeriesUrl: "https://api.themoviedb.org/3/search/tv?api_key=1003857666890380902d6e3595e8622a&language=it-IT&query=",
      
      filmsList: [],
      
      TVSeriesList: [],
      
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
          console.log(this.filmsList);
        }
      );

      axios.get(`${this.apiTVSeriesUrl}?api_key=${this.apiKey}&language=${this.currentLanguage}&query=${searchResult}`)
      .then((result) => {
        this.TVSeriesList = result.data.results;
        }
      );
    },

    checkResearch: function(value) {
      this.researchDone = value;
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
