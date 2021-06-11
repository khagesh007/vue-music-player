<template>
  <article class="screen">
    <input type="checkbox" value="None" id="magicButton" name="check" />
    <label class="main" for="magicButton"></label>

    <div class="coverImage"></div>
    <div class="search"></div>
    <div class="bodyPlayer"></div>

    <table class="list"  >
      <tr class="song" v-for="(song, index)  in songlist" :key="index">
        <td class="nr">
          <h5>{{index+1}}</h5></td>
        <td class="title"><h6>{{song.title}}</h6></td>
        <td class="length"><h5>3:54</h5></td>
        <td><input type="checkbox" id="heart"/><label class="zmr" for="heart"></label></td>
      </tr>


    </table>

    <div class="shadow"></div>
    <div id="progress-bar" class="bar flex-grow bg-white border border-blue-200"
    >
      <div class="overlay-container relative">
        <div>
        <span class="duration-played"  v-html="elapsedTime()"> 00:00 </span>
        <input
            v-model="playbackTime"
            type="range"
            min="0"
            :max="audioDuration"
            class="slider w-full h-full"
            id="position"
            name="position"
        />
        <span class="duration-total"  v-html="totalTime()"> 00:00 </span>
        </div>
        <!-- Show loading indicator until audio has been loaded -->
        <div v-show="!audioLoaded"
             class="w-full absolute top-0 bottom-0 right-0 left-0 px-2 pointer-events-none"
             style="color: #94bcec">
          Loading please wait...
        </div>

        <div
            v-show="audioLoaded"
            class="flex w-full justify-between absolute top-0 bottom-0 right-0 left-0 px-2 pointer-events-none"
        >





        </div>

      </div>
    </div>


    <div class="info">
      <h4>STRESSED OUT </h4>
      <h3>twenty one pilots - Blurryface</h3>
    </div>
    <audio preload="auto" id="audio" ref="audio" controls>
      <source src="https://cdn.pixabay.com/download/audio/2021/04/07/audio_8ed06844ef.mp3?filename=nightlife-michael-kobrin-95bpm-3783.mp3">
      <source src="http://www.jplayer.org/audio/mp3/Miaow-02-Hidden.ogg">
    </audio>
    <table class="player">
      <td><input type="checkbox" id="backward"/><label class="backward" for="backward"></label></td>
      <td><input type="checkbox" id="play" ref="play" title="Play" @click="togglePlayPause"/><label class="play" for="play"></label></td>
      <td><input type="checkbox" id="forward"/><label class="forward" for="forward"></label></td>
    </table>


    <table class="footer">
      <td><input type="checkbox" id="love" checked /><label class="love" for="love"></label></td>
      <td><input type="checkbox" id="shuffle"/><label class="shuffle" for="shuffle"></label></td>
      <td><input type="checkbox" id="repeat" checked /><label class="repeat" for="repeat"></label></td>
      <td><input type="checkbox" id="options"/><label class="options" for="options"></label></td>
    </table>

    <div class="current"><h2>STRESSED OUT</h2></div>


  </article>
</template>
<script>
export default {
  props : ['songlist'],
  data (){
    return {
      player :  this.$refs.play,
      playbackTime: 0,
      audioDuration: 100,
      audioLoaded: false,
      isPlaying: false
    }
  },
  methods : {

    togglePlayPause() {
      if (this.$refs.audio.paused || this.$refs.audio.ended) {
        this.$refs.play.title = "Pause";
        this.$refs.audio.play();
        this.isPlaying = true;
      } else {
        this.isPlaying = false;
        this.$refs.play.title = "Play";
        this.$refs.audio.pause();
      }
    },
    // toggleAudio() {
    //   var audio = this.$refs.audio;
    //   //var audio = document.getElementById("audio-player");
    //   if (audio.paused) {
    //     audio.play();
    //     this.isPlaying = true;
    //   } else {
    //     audio.pause();
    //     this.isPlaying = false;
    //   }
    // },
    //Set the range slider max value equal to audio duration
    initSlider() {
      var audio = this.$refs.audio;

      if (audio) {
        this.audioDuration = Math.round(audio.duration);
      }
    },
    //Convert audio current time from seconds to min:sec display
    convertTime(seconds){
      const format = val => `0${Math.floor(val)}`.slice(-2);
    //  let hours = seconds / 3600;
      let minutes = (seconds % 3600) / 60;
      return [minutes, seconds % 60].map(format).join(":");
    },
    //Show the total duration of audio file
    totalTime() {
      var audio = this.$refs.audio;
      if (audio) {
        var seconds = audio.duration;
        return this.convertTime(seconds);
      } else {
        return '00:00';
      }
    },
    //Display the audio time elapsed so far
    elapsedTime() {
      var audio = this.$refs.audio;
      if (audio) {
        var seconds = audio.currentTime;
        return this.convertTime(seconds);
      } else {
        return '00:00';
      }
    },
    //Playback listener function runs every 100ms while audio is playing
    playbackListener() {
      let audio = this.$refs.audio;
      //Sync local 'playbackTime' var to audio.currentTime and update global state
      this.playbackTime = audio.currentTime;

    //  console.log("update: " + audio.currentTime);
      //Add listeners for audio pause and audio end events
      audio.addEventListener("ended", this.endListener);
      audio.addEventListener("pause", this.pauseListener);
    },
    //Function to run when audio is paused by user
    pauseListener() {
      this.isPlaying = false;
      this.listenerActive = false;
      this.cleanupListeners();
    },
    //Function to run when audio play reaches the end of file
    endListener() {
      this.isPlaying = false;
      this.listenerActive = false;
      this.cleanupListeners();
    },
    //Remove listeners after audio play stops
    cleanupListeners() {
      var audio = this.$refs.audio;
      audio.removeEventListener("timeupdate", this.playbackListener);
      audio.removeEventListener("ended", this.endListener);
      audio.removeEventListener("pause", this.pauseListener);
      //console.log("All cleaned up!");
    }

  },
  mounted: function() {
    // nextTick code will run only after the entire view has been rendered
    this.$nextTick(function() {

      let audio=this.$refs.audio;
      //Wait for audio to load, then run initSlider() to get audio duration and set the max value of our slider
      // "loademetadata" Event https://www.w3schools.com/tags/av_event_loadedmetadata.asp
      audio.addEventListener(
          "loadedmetadata",
          function() {
            this.initSlider();
          }.bind(this)
      );
      // "canplay" HTML Event lets us know audio is ready for play https://www.w3schools.com/tags/av_event_canplay.asp
      audio.addEventListener(
          "canplay",
          function() {

            this.audioLoaded=true;
          }.bind(this)
      );
      //Wait for audio to begin play, then start playback listener function
      this.$watch("isPlaying",function() {

        if(this.isPlaying) {
          var audio=this.$refs.audio;
          this.initSlider();
          //console.log("Audio playback started.");
          //prevent starting multiple listeners at the same time
          if(!this.listenerActive) {
            this.listenerActive=true;
            //for a more consistent timeupdate, include freqtimeupdate.js and replace both instances of 'timeupdate' with 'freqtimeupdate'
            audio.addEventListener("timeupdate",this.playbackListener);
          }
        }
      });
      //Update current audio position when user drags progress slider
      this.$watch("playbackTime",function() {

        var diff=Math.abs(this.playbackTime-this.$refs.audio.currentTime);

        //Throttle synchronization to prevent infinite loop between playback listener and this watcher
        if(diff>0.01) {

          this.$refs.audio.currentTime=this.playbackTime;
        }
      });
    });
  }
}




</script>

<style>
@import '../assets/player.css';
</style>
