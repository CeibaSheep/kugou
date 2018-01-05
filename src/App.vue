<template>
    <div id="app">
         <Head />
         <div id="main">
             <keep-alive>
                 <rooter-view>
                     
                 </rooter-view>
             </keep-alive>
         </div>
         <Player />
         <DetailPlayer />
</template>

<script>
    import Head from './components/header/Head'
    import Player from './components/Player'
    import DetailPlayer from './components/DetailPlayer'
    export default {
        components: {
            Head,
            Player,
            DetailPlayer
        }
    }
</script>