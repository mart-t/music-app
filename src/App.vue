<template>
  <div id="app">
    <header>
      <h1>Musify</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">
          {{ current.title }} - <span>{{ current.artist }}</span>
        </h2>
        <div class="controls">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play">▶</button>
          <button class="pause" v-else @click="pause">❚❚</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>Playlist</h3>
        <button
          v-for="song in songs"
          :key="song.src"
          @click="play(song)"
          :class="song.src == current.src ? 'song playing' : 'song'"
        >
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: "Lights Up",
          artist: "Harry Styles",
          src: require("./assets/lights-up.mp3"),
        },
        {
          title: "Dark Necessities",
          artist: "Red Hot Chilly Peppers",
          src: require("./assets/dark-necessities.mp3"),
        },
        {
          title: "Astronaut in The Ocean",
          artist: "Masked Wolf",
          src: require("./assets/astronaut.mp3"),
        },
        {
          title: "Night Crawling",
          artist: "Miley Cyrus",
          src: require("./assets/night-crawling.mp3"),
        },
        {
          title: "Plastic Hearts",
          artist: "Miley Cyrus",
          src: require("./assets/plastic-hearts.mp3"),
        },
      ],
      player: new Audio(),
    };
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
        this.index = this.songs.indexOf(song);
      }
      this.player.play();
      this.player.addEventListener("ended", function () {
        if (this.index < this.songs.length - 1) {
          this.index++;
        } else {
          this.index = 0;
        }
        this.current = this.songs[this.index];
        this.play(this.current);
      }.bind(this));
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next() {
      if (this.index < this.songs.length - 1) {
        this.index++;
      } else {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev() {
      if (this.index >= 1) {
        this.index--;
      } else {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    this.player.play();
  },
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
  background-color: #212121;
  color: #fff;
}
main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}

.song-title{
  color: #53565A;
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
  justify-content:center;
  padding: 30px 15px;
}
button {
  appearance: none;
  background: none;
  border: 2px;
  outline: none;
  cursor: pointer;
}
button:hover {
  opacity: 0.8;
}
.play {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #FFF;
  background-color: darkcyan;
}
.next, .prev {
font-size: 12px;
font-weight: 500;
padding: 10px 20px;
margin: 0px 15px;
border-radius: 6px;
color: #FFF;
background-color: cornflowerblue;
}
.pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #FFF;
  background-color:dodgerblue;
}
.playlist {
  padding: 0px 30px;
}
.playlist h3 {
  color: #212121;
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}
.playlist .song {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}
.playlist .song:hover {
  color:darkcyan;
}
.playlist .song.playing {
  color: #FFF;
background-image:linear-gradient(to right, darkcyan, cornflowerblue) ;
}
</style>
