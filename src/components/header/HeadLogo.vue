<template>
    <div class="logo-container">
        <div class="head-logo">
            <a href="#">
                <img src="http://m.kugou.com/v3/static/images/index/logo.png" alt="">
            </a>
        </div>
        <div class="head-search" @click="searchRouter">
            <img src="http://m.kugou.com/v3/static/images/index/search.png" alt="">
        </div>
    </div>
</template>


<script>
    export default {
        methods: {
            searchRouter: function() {
                this.$store.commit('showDetailPlayer', false); //store.mutations.showDetailPlayer
                this.$router.push({
                    path: './search'
                })
            }
        }
    }
</script>