<template>
    <main>
        <CardElement v-for="film in films" :key="film.id"
        :film="film" />
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
            required: true,
            type: Array,
        }
    }

}
</script>

<style>

</style>