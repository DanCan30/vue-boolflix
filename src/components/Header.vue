<template>
    
    <header>

        <h1>dd/flix</h1>

        <div class="search-content">
            <label for="genre-filter">Filtra per genere:</label>
            <select name="genre-filter" id="genre-filter">

            </select>
            <input type="text" v-model.trim="searchInput" @keyup.enter="emitSearchResult(), emitSearchedValue()">
            <i class="fa-solid fa-magnifying-glass" @click="emitSearchResult(), emitSearchedValue()"></i>
        </div>
    </header>

</template>

<script>
export default {

    data: function() {
        return {
            searchInput: "",

            isSearched: false,
        }
    },

    methods: {
        emitSearchResult: function() {
            const result = this.$emit('searchInput', this.searchInput);
            this.isSearched = true;
            return result;
        },

        emitSearchedValue: function() {
            const value = this.$emit('isResearchDone', this.isSearched);
            return value;
        }
    }

}
</script>

<style lang="scss" scoped>

    header {
        height: 5rem;
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 2rem 1rem;

        h1 {
            color: red;
            text-transform: uppercase;
            font-size: 4rem;
            position: relative;

            &::after {
                content: "dd/flix";
                text-transform: uppercase;
                position: absolute;
                top: 8px;
                left: 8px;
                color: black;
                z-index: -1;

            }
        }

        div.search-content input {
            font-size: 2rem;
            margin: 0 1rem;
            outline: none;
            background-color: transparent;
            border: none;
            border-bottom: 2px solid red;
            color: white;
            transition: .4s;

            &:focus {
                border-color: white;
            }
        }


        div.search-content i {
            color: white;
            display: inline-block;
            margin-right: 2rem;
            font-size: 2rem;
            cursor: pointer;
            transition: .4s;

            &:hover {
                color: red;
            }
        }
    }

</style>