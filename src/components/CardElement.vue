<template>
  <div>


    <!-- <div>
        <h2 v-if="!film.title.toLowerCase().includes('undefined')">
            {{ film.title }}
        </h2>
        <ul v-if="!film.title.toLowerCase().includes('undefined')">
            <li>Titolo Originale: {{ film.original_title }} </li>
            <li>Lingua Originale: <img :src="flagUrl" :alt="film.original_language"></li>
            <li>Voto: {{ film.vote_average}} </li>
        </ul>
    </div> -->


    <div>
        <h2 v-if="!show.name.toLowerCase().includes('undefined')">
            {{ show.name }}
        </h2>
        <ul v-if="!show.name.toLowerCase().includes('undefined')">
            <li>Titolo Originale: {{ show.original_name }} </li>
            <li>Lingua Originale: <img :src="flagUrl" :alt="show.original_language"></li>
            <li>Voto: {{ show.vote_average}} </li>
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
        film: {
            type: Object,
        },

        show: {
            type: Object,
        }
    },

    methods: {
        getLanguageFlag: function() {
            axios.get(`${this.flagAPIUrl}${this.show.original_language}`)
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
                    default:
                        this.flagUrl = result.request.responseURL;
                    }
                }
            )

        },

        typeDetector: function() {

        }
    },

    created() {
        this.getLanguageFlag();
    }
}
</script>

<style lang="scss" scoped>
    img {
        width: 32px;
    }
</style>