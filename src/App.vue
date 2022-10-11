<template>
<div id="app">
  <header>
    <h1>My Music</h1>
  </header>
  <main>
    <section class="player">
      <h2 class="song-title">{{current.title}} - <span>{{current.artist}}</span></h2>

    <div class="contr">
      <button class="prev" @click="prev">Prev</button>
      <button class="play" v-if="!isPlaying" @click="play">Play</button>
      <button class="pause" v-else @click="pause">Pause</button>
      <button class="next" @click="next">Next</button>
    </div>
    </section>
    <section class="playlist">
      <h3>Playlist</h3>
      <button v-for="song in songs" :key="song.src" @click="play(song)" 
      :class="(song.src == current.src) ? 'song playing' :'song'">
      {{song.title}} - {{song.artist}}</button>
    </section>
  </main>
</div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return{
      current:{},
      index:0,
      isPlaying:false,
      songs:[
        {
          title:'Need To Know',
          artist:'Doja Cat',
          src: require('./assets/NeedToKnowDoja.mp3')
        },
        {
          title:'Mood',
          artist:'24K ft Iann Dior',
          src: require('./assets/Mood24k.mp3')
        },
        {
          title:'Family',
          artist:'David Guetta ft Dolla Sign',
          src: require('./assets/FamilyDollaSign.mp3')
        }
      ],
      player: new Audio()
    }
  },
  methods:{
    play(song){
      if(typeof song.src != "undefined"){
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener('ended',function(){
        this.index++;
      if(this.index > this.songs.length - 1){
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
      })
      this.isPlaying = true;
    },
    pause (){
      this.player.pause();
      this.isPlaying = false;
    },
    next(){
      this.index++;
      if(this.index > this.songs.length - 1){
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);

    },
    prev(){
        this.index--;
      if(this.index < 0){
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);

    }
  },
  created (){
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
   
  }
  
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: sans-serif;
}
header{
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: darkgrey;
  color:white;
}
main{
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}
.song-title{
  color: brown;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}
.song-title span{
  font-weight: 400;
  font-style: italic;

}
.contr{
  display: flex;
  justify-content: center;
  padding: 30px 15px;
  align-items: center;
}
button{
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}
.play , .pause{
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color:rgb(255, 255, 255);
  background-color: rgb(94, 0, 27);
}
button:hover{
  opacity: 0.8;
}
.next,.prev{
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color:rgb(255, 255, 255);
  background-color: rgb(238, 34, 92);
}
.playlist{
  padding: 0px 30px;
}
.playlist h3{
  color: darkslategray;
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
.playlist .song:hover{
  color: #FF5858;
}
.playlist .song.playing{
  color: thistle;
  background-image: linear-gradient(to right,#CC2E5D,#FF5858);
}
</style>
