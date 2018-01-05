<template>
    <div v-show="detailPlayerFlag">
        <div class="detail_player" :style="{backgroundImage:`url(${audio.imgUrl})`}"></div>
        <div class="detail_player" :style="{backgroundImage:`url(${audio.imgUrl})`,filter:'blur(5px)','-webkit-filter':'blur(5px)'}">
        </div>
        <div class="detail_player_content">
            <div class="detail_player_title container">
                <span class="detail_player_back" @click="hideDetailPlayer()"></span>
                {{audio.title}}
            </div>
            <div class="detail_player_img">
                <img :src="audio.imgUrl" alt="">
            </div>
            <div class="detail_player_lrc">
                <div class="lrc-content" :style="{marginTop:lrcOffset+'px'}">
                    <p v-for="(item,index) in songLrc" :class="{isCurrentLrc:item.seconds>=audio.currentLength}">
                        {{item.lrcContent}}
                    </p>
                </div>
            </div>
            <div class="detail_player_range container">
                <span class="detail_player_time">{{audio.currentLength|time}}</span>
                <mt-range
                    id="range"
                    :min="0"
                    :max="audio.songLength"
                    v-model="audio.currentLength"
                    :bar-height="3"
                    @click.native="rangeChange($event)" disable style="width: 80%">
                </mt-range>
                <span class="detail_player_time">{{audio.songLength|time}}</span>
            </div>
            <div class="detail_player_control player-padding">
                    <i class="detail_player_btn play-prev player_btn_sm" @click="prev()"></i>
                    <i class="detail_player_btn play-play player_btn_lg" @click="toggleStatus()" :class="{'play-pause':isPlay}"></i>
                    <i class="detail_player_btn play-next player_btn_sm" @click="next()"></i>
            </div>
        </div>
    </div>
</template>


<script>
    import {
        mapGetters
    } from 'vuex';
    export default {
        data() {
            return {
                rangeValue: 0
            }
        },
        filters: {
            time(value) {
                var length = Math.floor(parseInt(value));
                var minute = Math.floor(value / 60);
                if (minite < 10) {
                    minite = '0' + minite
                }
                var second = lenth % 60;
                if (second < 10) {
                    second = '0' + second;
                }
                return minite + ":" + second
            }
        },
        computed: {
            ...mapGetters(['audio', 'detailPlayerFlag', 'isPlay']), //store.states.{audio,detailPlayerFlag,isPlay}
            songLrc() {
                if (this.audio.lrc) {
                    var temp = this.audio.lrc.split('\r\n')
                    temp = temp.splice(0, temp.length - 1)
                    temp = temp.map((value) => {
                        var time = value.substr(1, 5)
                        var seconds = parseInt(time.split(':')[0]) * 60 + parseInt(time.split(':')[1])
                        var lrcContent = value.substr(10)
                        return {
                            seconds,
                            lrcContent
                        }
                    })
                    return temp;
                }
            },
            lrcOffset() {
                if (this.songLrc) {
                    var offset = (this.songLrc.length - document.querySelectorAll('.isCurrentLrc').length - 2) * (-20)
                    return this.audio.currentLength + offset - this.audio.currentLength
                }
            }
        },
        methods: {
            hideDetailPlayer() {
                this.$store.commit("showDetailPlayer", false) //store.mutations showDetailPlayer
            },
            rangeChange(event) {
                var offset = event.offsetX
                var rangeWidth = (document.documentElement.clientWidth - 20) * 0.8 - 20;
                var clickLength = Math.floor(offset * this.audio.songLength / rangeWidth)
                if (offset < rangeWidth) {
                    this.$store.commit('setAudioTime', clickLength)
                    this.$store.commit('setCurrent', true)
                }
            },
            toggleStatus() {
                if (this.isPlay) {
                    document.getElementById('audioPlay').pause()
                } else {
                    document.getElementById('audioPlay').play()
                }
                this.$store.commit('isPlay', !this.isPlay)
            },
            prev() {
                this.$store.dispatch('prev')
            },
            next() {
                this.$store.dispatch('next')
            }
        }
    }
</script>