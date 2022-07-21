<template>
    
    <header>

        <h1>netflix</h1>

        <div class="search">
            <input type="text" v-model.trim="searchInput" @keyup="emitSearchResult()">
            <i class="fa-solid fa-magnifying-glass" @click="emitSearchResult()"></i>
        </div>
    </header>

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
                content: "netflix";
                text-transform: uppercase;
                position: absolute;
                top: 8px;
                left: 8px;
                color: black;
                z-index: -1;

            }
        }

        div.search input {
            font-size: 2rem;
            margin: 0 1rem;
            outline: none;
            background-color: transparent;
            border: none;
            border-bottom: 2px solid white;
            color: white;
            transition: .4s;

            &:focus {
                border-color: red;
            }
        }


        div.search i {
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