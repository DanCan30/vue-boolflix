<template>
  <div id="app">

    <Header @searchInput="getShowsList"
    @isResearchDone="checkResearch"
    @genreFilter="genreFilter"
    :uniqueGenres="uniqueGenres" />
    <Main 
    
      :films="filteredFilmsList"
      :TVSeries="filteredTVSeriesList"
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

      selectedGenre: "all",

      filteredFilmsList: [],

      filteredTVSeriesList: [],
      
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

          if(this.selectedGenre === "all") {

            this.filteredFilmsList = this.filmsList


          } else {

            this.filteredFilmsList = this.filmsList.filter(film =>

              film.genre_ids.includes(this.selectedGenre)

            )
          }
        }
      );

      axios.get(`${this.apiTVSeriesUrl}?api_key=${this.apiKey}&language=${this.currentLanguage}&query=${searchResult}`)

      .then((result) => {
        this.TVSeriesList = result.data.results;

         if(this.selectedGenre === "all") {

          this.filteredTVSeriesList = this.TVSeriesList;

          } else {
            this.filteredTVSeriesList = this.TVSeriesList.filter(series =>

              series.genre_ids.includes(this.selectedGenre)

            )
          }
        }
      );

      axios.get(`${this.apiPopularSeriesUrl}?api_key=${this.apiKey}&language=${this.currentLanguage}`)

      .then((result) => {
        this.mostPopularShows = result.data.results;
        }
      );

    },

    checkResearch: function(value) {
      this.researchDone = value;
    },

    genreFilter: function(genre) {

      this.selectedGenre = genre;

    }
  },

  computed: {
    
    getUniqueGenres: function() {
      axios.get(`${this.apiFilmsGenresUrl}?api_key=${this.apiKey}&language=${this.currentLanguage}`)
      .then((result) => {

        this.uniqueGenres = result.data.genres;
        
      });

    },
  },

  created() {
    this.getShowsList();
    this.getUniqueGenres;
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
