<template>
  <div id="app">
    <header>
      <h1>Nadio's Music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">{{current.title}} - <span>{{current.artist}}</span></h2>
        <div class="control">
          <button class="previous" @click="previous()"> Previous</button>
          <button v-if="!isPlaying" class="play" @click="play(current)">Play > </button>
          <button v-if="isPlaying" class="pause" @click="pause()">|| Pause</button>
          <button class="next" @click="next()">Next >></button>
        </div>
      </section>
      <section class="playlist">
        <h3>Your playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src === current.src) ? 'songPlaying' : 'song'">
          {{song.title}} - {{song.artist}}
        </button>
      </section>
    </main>
    
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      current:{},
      index:0,
      isPlaying: false,
      songs:[
        {
          title:'All I Want',
          artist:'Kodaline',
          src:require('./assets/All_I_Want.mp3')
        },
        {
          title:'Laal Ishq',
          artist:'Arijit Singh',
          src:require('./assets/Laal_Ishq.mp3')
        },
        {
          title:'Iktara',
          artist:'Tochi raina',
          src:require('./assets/Iktara.mp3')
        }
      ],
      player: new Audio()
    }
  },
  methods:{
    play(song) {
      if(typeof song.src !== undefined) {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener['ended',function (){
        this.index++;
        if(this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
      }.bind(this)];
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    previous() {
      this.index--;
      if(this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    next() {
      this.index++;
      if(this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  },
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: Roboto,RobotoDraft,Helvetica,Arial,sans-serif;
}
header{
  display:flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background: #212121;
  color: #fff;
}
main{
  width:100%;
  margin:0 auto;
  max-width:768px;
  padding:25px;
}
.song-title{
  color:#212121;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}
.song-title span{
  font-weight: 500;
  font-style: italic;

}
.control{
  display:flex;
  justify-content: center;
  padding: 30px 50px;;
  align-items: center;
}
button{
  appearance: none;
  background: none;
  border: none;
  cursor: pointer;
}
button:hover{
  opacity: 0.8;
}
.play{
  font-size: 20px;
  padding: 15px 25px;
  margin:0px 15px;
  border-radius: 8px;
  color:#FFF;
  background-color:#cc2e5d
}
.pause{
  font-size: 20px;
  padding: 15px 25px;
  margin:0px 15px;
  border-radius: 8px;
  color:#FFF;
  background-color:#cc2e5d
}
.previous,.next{
  font-size: 16px;
  padding: 10px 20px;
  margin:0px 15px;
  border-radius: 6px;
  color:#FFF;
  background-color:#ff5858
}

.playlist{
  padding: 0px 30px;
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}
.playlist .song{
  display: block;
  padding: 15px;
  width:100%;
  font-size: 20px;
  font-weight: 300;
  cursor: pointer;
}
.playlist .songPlaying{
  display: block;
  padding: 15px;
  width:100%;
  font-size: 20px;
  font-weight: 300;
  cursor: pointer;
  color:#FFF;
  background-image:linear-gradient(to right,#cc2e5d,#ff585d)
}
.playlist .song:hover{
  background: #ff585d;
}

</style>
