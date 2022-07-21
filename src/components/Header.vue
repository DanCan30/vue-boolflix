<template>
    
    <div>
        <input type="text" v-model="searchInput">
        <button @click="$emit('search', searchInput)">Cerca</button>
    </div>

</template>

<script>
export default {

    data: function() {
        return {
            searchInput: "",
        }
    }

}
</script>

<style>

</style>