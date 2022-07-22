<template>
  <div>
    <div class="card" v-if="item.title ? (!item.title.toLowerCase().includes('undefined')) : (!item.name.toLowerCase().includes('undefined')) ">

        <div class="card-flipper">

            <div class="card-front">
                <img class="poster" :src="(item.poster_path == null ) ? require('../assets/img/Poster-not-found.jpg') : `https://image.tmdb.org/t/p/w342${item.poster_path}` " :alt="`${ item.title ? item.title : item.name }'s poster`">
            </div>

            <div class="card-back">

                <h2>
                    {{ item.title ? item.title : item.name }}
                </h2>
                <ul>
                    <li v-if="(item.title !== item.original_title) || ( item.name !== item.original_name )">Titolo Originale: {{ item.original_title ? item.original_title : item.original_name }} </li>
                    <li>Lingua Originale: <img class="language-flag" :src="flagUrl" :alt="item.original_language"></li>
                    <li>
                        Voto: 
                        <i class="fa-solid fa-star active-star" v-for="(lightStar, index) in getRatingOutOfFive(item.vote_average)" :key="index"></i>
                        <i class="fa-solid fa-star" v-for="(darkStar, index) in 5 - getRatingOutOfFive(item.vote_average)" :key=" '0' + index"></i>
                    </li>
                    <li>Genere: <span v-for="(genre, index) in item.genre_ids" :key="index"> {{ getGenresFromID(item.genre_ids[index]) }} - </span></li>
                </ul>

            </div>

        </div>
    </div>
        
  </div>
</template>

<script>

import axios from "axios";

export default {

    data: function() {
        return {

            flagAPIUrl: "https://countryflagsapi.com/png/",
            flagUrl: "",
        }
    },

    props: {
        item: {
            type: Object,
        },

        genres: {
            type: Array,
        }

    },

    methods: {
        getLanguageFlag: function() {
            axios.get(`${this.flagAPIUrl}${this.item.original_language}`)
            .then((result) => {

                    this.flagUrl = result.request.responseURL;

                    switch(this.flagUrl) {
                    case 'https://countryflagsapi.com/png/en':
                        this.flagUrl = 'https://countryflagsapi.com/png/gb';
                        break;
                    case 'https://countryflagsapi.com/png/zh':
                        this.flagUrl = 'https://countryflagsapi.com/png/cn';
                        break;
                    case 'https://countryflagsapi.com/png/ja':
                        this.flagUrl = 'https://countryflagsapi.com/png/jp';
                        break;
                    case 'https://countryflagsapi.com/png/el':
                        this.flagUrl = 'https://countryflagsapi.com/png/sv';
                        break;
                    case 'https://countryflagsapi.com/png/ko':
                        this.flagUrl = 'https://countryflagsapi.com/png/kr';
                        break;
                    case 'https://countryflagsapi.com/png/he':
                        this.flagUrl = 'https://countryflagsapi.com/png/il';
                        break;
                    case 'https://countryflagsapi.com/png/ur':
                        this.flagUrl = 'https://countryflagsapi.com/png/pk';
                        break;
                    default:
                        this.flagUrl = result.request.responseURL;
                    }
                }
            )

        },

        lowerCaseElement: function(element) {
            return element.toLowerCase();
        },

        getRatingOutOfFive: function(value) {

            let rating = Math.ceil(value / 2);
            return rating;

        },

        getGenresFromID: function(genreID) {

            this.genres.forEach((genre) => {

                if(genre.id == genreID) {
                    console.log(genre.name);
                    return genre.name;
                };
                
            });

        },

    },

    created() {
        this.getLanguageFlag();
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


        img.language-flag {
            width: 32px;
        }

        .active-star {
            color: yellow;
        }

        &:hover .card-flipper {
            transform: rotateY(180deg);
        }

    }


</style>