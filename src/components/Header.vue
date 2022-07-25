<template>
    
    <header>

        <h1>dd/flix</h1>

        <div class="search-content">
            <label for="genre-filter">Filtra per genere:</label>
            <select name="genre-filter" id="genre-filter" @change="emitGenreFilter()" v-model="genreInput">
                <option value="all">Tutti</option>
                <option v-for="(genre, index) in uniqueGenres" :key="index" :value="uniqueGenres[index].id">{{ uniqueGenres[index].name }}</option>
            </select>
            <input type="text" v-model.trim="searchInput" @keyup.enter="emitSearchResult(), emitSearchedValue()">
            <i class="fa-solid fa-magnifying-glass" @click="emitSearchResult(), emitSearchedValue()"></i>
        </div>
    </header>

</template>

<script>
export default {

    props: {
        uniqueGenres: {
            type: Array,
        },
    },

    data: function() {
        return {
            searchInput: "",

            genreInput: "all",

            isSearched: false,
        }
    },

    methods: {
        emitSearchResult: function() {
            const result = this.$emit('searchInput', this.searchInput);
            this.isSearched = true;
            return result;
        },
        
        emitGenreFilter: function() {
            const result = this.$emit('genreFilter', this.genreInput);
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

        div.search-content label {
            font-size: 2rem;
            text-transform: uppercase;
            color: white;
        }

        div.search-content select {
            margin: 0 3rem 0 1rem;
            width: 20rem;
            font-size: 2rem;
            background-color: transparent;
            border: none;
            border-bottom: 2px solid red;
            color: white;
            cursor: pointer;

            &:focus {
                border: none;
            }

            option {
                cursor: pointer;
                background-color: black;
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