<template>
  <div id="app">
    <header>
      <h1>My music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">{{current.title}} -  <span>{{current.artist}}</span></h2>
        <div class="control">
          <button class="prev" v-on:click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" v-on:click="play">Play</button>
          <button class="pause" v-else v-on:click="pause">Pause</button>
          <button class="next"  v-on:click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>The Playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" 
        :class="(song.src == current.src) ? 'song playing' : 'song' ">

        {{song.title}} - {{song.artist}}

        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name:'app',
  data(){
    return{
      current:{},
      index: 0,
      isPlaying: false,
      songs:[
        {title:'Ginga Girigiri!!', artist: 'DBZ', src: require('./assets/Dragon Ball Z - Sad Song.mp3')},
        {title:'Sadness and Sorrow', artist: 'Naruto', src: require('./assets/Naruto Soundtrack- Sadness and Sorrow (FULL VERSION).mp3')}
      ],
      player: new Audio()
    }
  },
  methods:{
    play: function(song){
      if(typeof song.src != "undefined"){
        this.current = song;
        this.player.src = this.current.src;
      }

      this.player.play();
      this.isPlaying = true;
    },

    pause: function(){
      this.player.pause();
      this.isPlaying= false;
    },

    prev: function(){
      this.index--;
      if(this.index < 0){
        this.index = this.songs.length - 1;
      }

      this.play(this.songs[this.index]);
    },

    next: function(){
      this.index++;

      if(this.index > this.songs.length - 1){
        this.index = 0;
      }

       this.play(this.songs[this.index]);
    }

  },
  created(){
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
}

header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: black;
  color: white;
}

</style>