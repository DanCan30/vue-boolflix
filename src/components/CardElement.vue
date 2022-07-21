<template>
  <div>
    <div v-if="item.title ? (!item.title.toLowerCase().includes('undefined')) : (!item.name.toLowerCase().includes('undefined')) ">

        <img :src="(item.poster_path == null ) ? require('../assets/img/Poster-not-found.jpg') : `https://image.tmdb.org/t/p/w342${item.poster_path}` " :alt="`${ item.title ? item.title : item.name }'s poster`">


        <h2>
            {{ item.title ? item.title : item.name }}
        </h2>
        <ul>
            <li>Titolo Originale: {{ item.original_title ? item.original_title : item.original_name }} </li>
            <li>Lingua Originale: <img class="language-flag" :src="flagUrl" :alt="item.original_language"></li>
            <li>Voto: {{ item.vote_average ? item.vote_average : item.vote_average }} </li>
        </ul>

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
                    default:
                        this.flagUrl = result.request.responseURL;
                    }
                }
            )

        },

        lowerCaseElement: function(element) {
            return element.toLowerCase();
        }
    },

    created() {
        this.getLanguageFlag();
    }
}
</script>

<style lang="scss" scoped>
    img.language-flag {
        width: 32px;
    }
</style>