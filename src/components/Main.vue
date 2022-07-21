<template>
    <main>
        <div>
            <h2>film</h2>
            <CardElement v-for="film in films" :key="film.id"
            :item="film" />
        </div>

        <div>
            <h2>serie tv</h2>
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

<style>

</style>