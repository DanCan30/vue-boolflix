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

      apiGenresUrl: "https://api.themoviedb.org/3/genre/movie/list",

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

        // Get Urls for axios requests based on the user research
      const filmsUrl = axios.get(this.apiFilmsUrl + "?api_key=" + this.apiKey + "&language=" + this.currentLanguage + "&query=" + searchResult);
      const TVSeriesUrl = axios.get(this.apiTVSeriesUrl + "?api_key=" + this.apiKey + "&language=" + this.currentLanguage + "&query=" + searchResult);
      const popularUrl = axios.get(this.apiPopularSeriesUrl + "?api_key=" + this.apiKey + "&language=" + this.currentLanguage);

        // Use a single axios request for all the requests
      axios.all([filmsUrl, TVSeriesUrl, popularUrl]).then(axios.spread((...responses) =>
      {
          // Spread the responses in different arrays
        const films = responses[0].data.results;
        const TVSeries = responses[1].data.results;
        const popular = responses[2].data.results;

        this.filmsList = films;
        this.TVSeriesList = TVSeries;
        this.mostPopularShows = popular;

          // If the genre filter is setted on "all" the filtered lists are equal to the normal lists
        if(this.selectedGenre === "all") {

          this.filteredFilmsList = this.filmsList;
          this.filteredTVSeriesList = this.TVSeriesList;

        } else {

            // Else filter the array by the selected genre
          this.filteredFilmsList = this.filmsList.filter(film =>
            film.genre_ids.includes(this.selectedGenre)
          );

          this.filteredTVSeriesList = this.TVSeriesList.filter(show =>
            show.genre_ids.includes(this.selectedGenre)
          );
        }
      }));

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
      axios.get(`${this.apiGenresUrl}?api_key=${this.apiKey}&language=${this.currentLanguage}`)
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
