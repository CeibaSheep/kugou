<template>
    <div>
        <mt-swipe :auto="5000">
            <mt-swipe-item v-for="(banner,index) in banners" :key="index">
                <a :href="banner.extra.tourl">
                    <img :src="banner.imgUrl" :title="banner.title" alt="">
                </a>
            </mt-swipe-item>
        </mt-swipe>
        <mt-cell v-for="(song,index) in songList" :title="song.filename" @click.native="playAudio(index)" :key="index">
            <img src="../assets/images/download_icon.png" width="20" height="20" alt="">
        </mt-cell>
    </div>
</template>



<script>
    import {
        Indicator
    } from 'mint-ui'
    import {
        PLAY_AUDIO
    } from '../mixins'

    export default {
        mixins: [PLAY_AUDIO],
        data() {
            return {
                banners: [],
                songList: []
            }
        },
        created() {
            this.getSongs()
        },
        methods: {
            getSongs() {
                Indicator.open({
                    text: '加载中',
                    spinnerType: 'snake'
                });
                this.$http.get('/proxy/?json=true').then(({
                    data
                }) => {
                    this.banners = data.banner
                    this.songList = data.data
                }).then(() => {
                    Indicator.close()
                })
            },
        }
    }
</script>