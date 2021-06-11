<template>
  <div id="app">

    <PlayerHome :songlist="songList"/>
  </div>
</template>

<script>
import PlayerHome from './components/PlayerHome.vue'

export default {
  name: 'App',
  data(){
    return {
      songList : [
        {title : 'Night life', artist : 'MichaelKobrin', url : 'https://cdn.pixabay.com/download/audio/2021/04/07/audio_8ed06844ef.mp3?filename=nightlife-michael-kobrin-95bpm-3783.mp3'},
        {title : 'Under pressure', artist : 'MichaelKobrin', url : 'https://cdn.pixabay.com/download/audio/2021/04/07/audio_d5755615b6.mp3?filename=under-pressure-michael-kobrin-105bpm-3781.mp3'},
        {title : 'Canción Triste', artist : 'enrique27naveda', url : 'https://cdn.pixabay.com/download/audio/2020/11/10/audio_547ebbf828.mp3?filename=cancion-triste-1502.mp3'},
        {title : 'Cali', artist : 'Wataboi', url : 'https://cdn.pixabay.com/download/audio/2020/10/11/audio_fe4d3bcac9.mp3?filename=cali-1171.mp3'},
        {title : 'Deep Music Every Day', artist : 'DeepMusicEveryDay', url : 'https://cdn.pixabay.com/download/audio/2021/06/10/audio_2db476ae90.mp3?filename=deep-music-every-day-kingdom-5015.mp3'},
        {title : 'Future Bass Atmospheric', artist : 'jorikbasov', url : 'https://cdn.pixabay.com/download/audio/2021/06/07/audio_28d9e827de.mp3?filename=future-bass-atmospheric-4954.mp3'},
        {title : 'Dreamy Piano Soft Sound', artist : 'WinkingFoxMusic', url : 'https://cdn.pixabay.com/download/audio/2021/04/21/audio_1897ccbea1.mp3?filename=dreamy-piano-soft-sound-ambient-background-4049.mp3'},
        {title : 'Night life', artist : 'MichaelKobrin', url : 'https://cdn.pixabay.com/download/audio/2021/04/07/audio_8ed06844ef.mp3?filename=nightlife-michael-kobrin-95bpm-3783.mp3'},
        {title : 'Under pressure', artist : 'MichaelKobrin', url : 'https://cdn.pixabay.com/download/audio/2021/04/07/audio_d5755615b6.mp3?filename=under-pressure-michael-kobrin-105bpm-3781.mp3'},
      ]
  }
  },
  components: {
    PlayerHome
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
