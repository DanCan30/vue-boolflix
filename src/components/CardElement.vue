<template>
  <div>
    <h2>
        {{ (film.title.toLowerCase().includes("undefined")) ? "" : film.title }}
    </h2>

    <ul v-if="!film.title.toLowerCase().includes('undefined')">
        <li>Titolo: {{ film.title }}</li>
        <li>Titolo Originale: {{ film.original_title }} </li>
        <li>Lingua Originale: {{ film.original_language }} </li>
        <li>Voto: {{ film.vote_average }} </li>
    </ul>
  </div>
</template>

<script>
export default {

    props: {
        film: {
            required: true,
            type: Object,
        }
    }
}
</script>

<style>

</style>