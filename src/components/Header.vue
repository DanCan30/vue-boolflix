<template>
    
    <div>
        <input type="text" v-model.trim="searchInput" @keyup.enter="emitSearchResult()">
        <button @click="emitSearchResult()">Cerca</button>
    </div>

</template>

<script>
export default {

    data: function() {
        return {
            searchInput: "",
        }
    },

    methods: {
        emitSearchResult: function() {
            const result = this.$emit('search', this.searchInput);
            return result;
        }
    }

}
</script>

<style>

</style>