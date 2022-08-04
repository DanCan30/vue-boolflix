<template>
  <div>
    <div class="card" v-if="(!valueCheck(item.title, item.name).toLowerCase().includes('undefined'))" >

        <div class="card-flipper">

            <div class="card-front">
                <img class="poster" :src="(item.poster_path == null ) ? require('../assets/img/Poster-not-found.jpg') : `https://image.tmdb.org/t/p/w342${item.poster_path}` " :alt="`${ valueCheck(item.title, item.name) }'s poster`">
            </div>

            <div class="card-back">

                <h2>
                    {{ valueCheck(item.title, item.name) }}
                </h2>
                <ul>
                    <li v-if="(item.title !== item.original_title) || ( item.name !== item.original_name )">Titolo Originale: {{ valueCheck(item.original_title, item.original_name) }} </li>
                    <li>Lingua Originale: <lang-flag :iso="item.original_language" /></li>
                    <li>
                        Voto: 
                        <i class="fa-solid fa-star active-star" v-for="(lightStar, index) in getRatingOutOfFive(item.vote_average)" :key="index"></i>
                        <i class="fa-solid fa-star" v-for="(darkStar, index) in 5 - getRatingOutOfFive(item.vote_average)" :key=" '0' + index"></i>
                    </li>
                    <li v-if="item.genre_ids.length != 0">Genere: - <span v-for="(genre, index) in item.genre_ids" :key="index"> {{ getGenresFromID(item.genre_ids[index]) }} - </span></li>
                    <li v-else >Genere: - {{ undefinedElement }} -</li>
                    <li>Cast:
                        <div v-if="item.title">

                            <span  v-for="(actor, index) in filmsCast.slice(0, -1)" :key="index"> {{ actor }},</span>
                            <span> {{ " " + filmsCast[filmsCast.length-1] }} </span> 

                        </div>
                        <div v-else-if="item.name">

                            <span v-for="(actor, index) in TVSeriesCast.slice(0, -1)" :key="index"> {{ actor }}, </span>
                            <span> {{ " " + TVSeriesCast[TVSeriesCast.length - 1] }} </span> 

                        </div>
                        
                    
                    </li>
                </ul>

            </div>

        </div>
    </div>
        
  </div>
</template>

<script>

import axios from "axios";
import LangFlag from 'vue-lang-code-flags';

export default {

    components: {
        LangFlag,
    },

    data: function() {
        return {

            apiKey: "1003857666890380902d6e3595e8622a",


            filmsCastApiUrl: "https://api.themoviedb.org/3/movie/",
            TVSeriesCastApiUrl: "https://api.themoviedb.org/3/tv/",

            filmsCast: [],
            TVSeriesCast: [],

            undefinedElement: "N/A",


        }
    },

    props: {
        item: {
            type: Object,
        },

        genres: {
            type: Array,
        },

    },

    methods: {
        
        valueCheck: function(value1, value2) {

            return value1 ? value1 : value2;
        },

        getRatingOutOfFive: function(value) {

            return Math.ceil(value / 2);

        },

        getGenresFromID: function(genreID) {

            let currentGenre = "";

            this.genres.forEach((genre) => {

                if(genre.id == genreID) {
                    currentGenre = genre.name;
                };

            });

            return currentGenre;

        },

        
        getCastElements: function() {

            
            axios.get(`${this.TVSeriesCastApiUrl}${this.item.id}/credits?api_key=${this.apiKey}`)
            .then((result) => {

                let castNumber = result.data.cast.length;
                
                if(castNumber > 5) {
                    castNumber = 5;
                };
                
                if(castNumber != 0) {

                    for (let i = 0; i < castNumber; i++) {
                        this.TVSeriesCast.push(result.data.cast[i].name)
                    };
                    
                } else {
                    this.TVSeriesCast.push(this.undefinedElement);

                }

            },
            ) .catch((error) => {
                
                if(error.response.status === 404) {
                    console.warn("Cast element is missing");
                }
               
            });
        
            axios.get(`${this.filmsCastApiUrl}${this.item.id}/credits?api_key=${this.apiKey}`)
            .then((result) => {

                let castNumber = result.data.cast.length;

                if(castNumber > 5) {
                    castNumber = 5;
                };

                if(castNumber != 0) {

                    for (let i = 0; i < castNumber; i++) {
                        this.filmsCast.push(result.data.cast[i].name)
                    };
                
                } else {
                    this.filmsCast.push(this.undefinedElement);

                }

                },
                
            ) .catch((error) => {
                
                if(error.response.status === 404) {
                    console.warn("Elemento non trovato");
                }
               
            });
            
            
        },

    },

    created() {
        this.getCastElements();
    }
}
</script>

<style lang="scss" scoped>

    div.card {

        
        width: 20rem;
        height: 30rem;
        margin: 0 1rem;
        cursor: pointer;
        perspective: 1000px;
        text-transform: uppercase;

        .card-flipper {
            transform-style: preserve-3d;
            width: 100%;
            height: 100%;
            position: relative;
            transition: transform .3s;
        }

        .card-front {

            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;

            img.poster {
                width: 100%;
                height: 100%;
                object-fit: cover;

            }
        }

        .card-back {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: black;
            color: white;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            padding: 1rem;
            backface-visibility: hidden;
            transform: rotateY(180deg);
            border: 10px solid white;

            ul {
                list-style: none;
            }

        }

        .active-star {
            color: yellow;
        }

        &:hover .card-flipper {
            transform: rotateY(180deg);
        }

    }


</style>