<template>
  <div id="app">
    <header>
      <h1>My music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">
          {{current.title}} -
          <span>{{current.artist}}</span>
        </h2>
        <div class="controls">
          <button class="prev" v-on:click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" v-on:click="play">Play</button>
          <button class="pause" v-else v-on:click="pause">Pause</button>
          <button class="next" v-on:click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>The Playlist</h3>
        <button
          v-for="song in songs"
          :key="song.src"
          @click="play(song)"
          :class="(song.src == current.src) ? 'song playing' : 'song' "
        >{{song.title}} - {{song.artist}}</button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: "Ginga Girigiri!!",
          artist: "DBZ",
          src: require("./assets/Dragon Ball Z - Sad Song.mp3")
        },
        {
          title: "Sadness and Sorrow",
          artist: "Naruto",
          src: require("./assets/Naruto Soundtrack- Sadness and Sorrow (FULL VERSION).mp3")
        }
      ],
      player: new Audio()
    };
  },
  methods: {
    play: function(song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }

      this.player.play();
      this.player.addEventListener(
        "ended",
        function() {
          this.index++;

          if (this.index > this.songs.length - 1) {
            this.index = 0;
          }

          this.play(this.songs[this.index]);
        }.bind(this)
      );
      this.isPlaying = true;
    },

    pause: function() {
      this.player.pause();
      this.isPlaying = false;
    },

    prev: function() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }

      this.play(this.songs[this.index]);
    },

    next: function() {
      this.index++;

      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }

      this.play(this.songs[this.index]);
    }
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  }
};
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

main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}

.song-title {
  color: #53565a;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}

.song-title span {
  font-weight: 400;
  font-style: italic;
}

.controls {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 15px;
}

button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}

button:hover {
  opacity: 0.8;
}
.play, .pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: white;
  background-color: #cc2e5d;
}

.next, .prev{
    font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: white;
  background-color: #ff5858;
}

.playlist{
  padding: 0px 30px;
}

.playlist h3{
  color: #212121;
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}

.playlist .song{
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}

.playlist .song:hover{
  color: #ff5858;
}

.playlist .song.playing{
  color: white;
  background-image: linear-gradient(to right, #cc2e50, #ff5858)
}
</style>