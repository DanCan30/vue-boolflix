<template>
    <main>
        <h2 class="error-message" v-if="(films.length === 0) && (TVSeries.length === 0) && (researchDone)">Nessun risultato trovato.</h2>

        <h2 class="film-title" v-if="films.length > 2">films</h2>
        <div class="cards-container">
            <CardElement v-for="film in films" :key="film.id"
            :item="film"
            :genres="filmsGenre"
            />
        </div>

        <h2 class="tv-show-title" v-if="TVSeries.length > 0">serie tv</h2>
        <div class="cards-container">
            <CardElement v-for="show in TVSeries" :key="show.id"
            :item="show"
            :genres="TVShowsGenre"
            />
        </div>

        <div class="search-message-wrapper">
            <h2 class="search-message" v-if="!researchDone">Cerca qualcosa!</h2>
        </div>


        <h2 class="popular-show-title" v-if="!researchDone">i titoli del momento</h2>
        <div class="cards-container" v-if="!researchDone">
            <CardElement v-for="popularShow in popular" :key="popularShow.id"
            :item="popularShow" 
            :genres="TVShowsGenre"
            />
        </div>

    </main>
</template>

<script>

import CardElement from "./CardElement.vue";
import axios from "axios";

export default {

    data: function() {
        return {

            currentLanguage: "it-IT",

            apiKey: "1003857666890380902d6e3595e8622a",
        
            filmsGenreUrl: "https://api.themoviedb.org/3/genre/movie/list",
            TVShowsGenreUrl: "https://api.themoviedb.org/3/genre/tv/list",
            
            filmsGenre: [],
            TVShowsGenre: [],

        }
    },  


    components: {
        CardElement,
    },

    props: {
        films: {
            type: Array,
        },

        TVSeries: {
            type: Array,
        },

        popular: {
            type: Array,
        },

        researchDone: {
            type: Boolean,
        }
    },

    methods: {
        
        getGenres: function() {
            axios.get(`${this.filmsGenreUrl}?api_key=${this.apiKey}&language=${this.currentLanguage}`)
            .then((result) => {
                this.filmsGenre = result.data.genres;
            })

            axios.get(`${this.TVShowsGenreUrl}?api_key=${this.apiKey}&language=${this.currentLanguage}`)
            .then((result) => {
                this.TVShowsGenre = result.data.genres;
            })
        },
        

    },

    created() {
        this.getGenres();
    }

}
</script>

<style lang="scss" scoped>

    h2.error-message {
        color: red;
        text-align: center;
        margin-top: 5rem;
    }
    
    h2 {
        text-transform: uppercase;
        margin-left: 2rem;
        font-size: 3rem;
        color: red;
        position: relative;

        &.film-message {
            margin-top: 1rem;
        }

        &.film-title::after {
            content: "films";
            text-transform: uppercase;
            position: absolute;
            top: 8px;
            left: 8px;
            color: black;
            z-index: -1;
        }

        &.tv-show-title::after {
            content: "serie tv";
            text-transform: uppercase;
            position: absolute;
            top: 8px;
            left: 8px;
            color: black;
            z-index: -1;
        }
        &.search-message::after {
            content: "Cerca qualcosa!";
            width: 100%;
            text-transform: uppercase;
            position: absolute;
            top: 8px;
            left: 8px;
            color: black;
            z-index: -1;
        }
    }

    div.cards-container {
        margin: 1rem 2rem;
        overflow-x: auto;
        overflow-y: hidden;
        display: flex;

        &::-webkit-scrollbar {
            display: none;
        }
    }

    .search-message-wrapper {
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .popular-show-title {
        margin-top: 4rem;
    }


</style>