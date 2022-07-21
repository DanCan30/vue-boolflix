<template>
  <div id="app">

    <Header @search="getFilmsList" />

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
      apiUrl: "https://api.themoviedb.org/3/search/movie?api_key=1003857666890380902d6e3595e8622a&query="
    }
  },

  components: {
    Header,
    Main,
  },
  methods: {
    getFilmsList: function(search) {
      axios.get(`${this.apiUrl}${search}`)
      .then((result) => 
      console.log(result.data.results));
    }
  },

  created() {
    this.getFilmsList();
  }
}
</script>

<style lang="scss">

</style>
