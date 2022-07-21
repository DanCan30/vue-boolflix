<template>
    <main>
        <h2 class="film-wrapper" v-if="films.length > 2">films</h2>
        <div class="cards-container">
            <CardElement v-for="film in films" :key="film.id"
            :item="film" />
        </div>

        <h2 class="tv-show-wrapper" v-if="TVSeries.length > 0">serie tv</h2>
        <div class="cards-container">
            <CardElement v-for="show in TVSeries" :key="show.id"
            :item="show" />
        </div>
    </main>
</template>

<script>

import CardElement from "./CardElement.vue"

export default {

    components: {
        CardElement,
    },

    props: {
        films: {
            type: Array,
        },

        TVSeries: {
            type: Array,
        }
    }

}
</script>

<style lang="scss" scoped>
    
    h2 {
        text-transform: uppercase;
        margin-left: 2rem;
        font-size: 3rem;
        color: red;
        position: relative;

        &.film-wrapper {
            margin-top: 1rem;
        }

        &.film-wrapper::after {
            content: "films";
            text-transform: uppercase;
            position: absolute;
            top: 8px;
            left: 8px;
            color: black;
            z-index: -1;
        }

        &.tv-show-wrapper::after {
            content: "serie tv";
            text-transform: uppercase;
            position: absolute;
            top: 8px;
            left: 8px;
            color: black;
            z-index: -1;
        }

    }

    div.cards-container {
        margin: 2rem;
        overflow-x: auto;
        overflow-y: hidden;
        display: flex;

        &::-webkit-scrollbar {
            display: none;
        }
    }



</style>