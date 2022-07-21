<template>
    <main>
        <!-- <CardElement v-for="film in films" :key="film.id"
        :film="film" /> -->
        <CardElement v-for="show in TVSeries" :key="show.id"
        :show="show" />
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