<template>
    <main>
        <h2>film</h2>
        <div class="cards-container">
            <CardElement v-for="film in films" :key="film.id"
            :item="film" />
        </div>

        <h2>serie tv</h2>
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
    
    div.cards-container {
        margin: 2rem;
        overflow-x: auto;
        overflow-y: hidden;
        display: flex;
    }

</style>