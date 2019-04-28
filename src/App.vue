<template>
  <div id="app">
    <h1>吉他和弦表生成器</h1>
    <div class='three input'>
      <input placeholder='C' v-model="chordName" @input='getChordString' type="input">
      <input v-model='chordString'  disabled type="input" style="display:none" >
    </div>
      <Canvas :gtpSetting="gtpSetting">
        <Guitar :chordName="chordName" :chordString="chordString" :gtpSetting="gtpSetting"/>
      </Canvas>
  </div>
</template>

<script>
import Canvas from './components/Canvas.vue'
import Guitar from './components/Guitar.vue'
const chordTable = {
  // C key
  C: [ 'o1o23x' ],
  Cmaj7: [ 'ooo2:13:2x' ],
  Dm: [ '132oxx' ],
  Em: [ 'ooo2:32o' ],
  F: [ '1123:431' ],
  G: [ '3:4ooo23', '3ooo2:13:2' ],
  Am: [ 'o12:32ox' ],
  G7: [ '1ooo23' ],
  E7: [ 'o3:412:32o' ],
  //D key
  A7: [ 'o2:3o2ox' ],
  Bm: [ '2:13:244:32:1x' ],
  A: [ 'o2:322:1ox' ],
  //G: [ '3:4ooo23', '3ooo2:13:2' ],
  '#Fm': [ '2:12:12:144:32:1'],
  //'Em': [ 'ooo2:32o' ],
  D: [ '2:13:22:1oxx' ],
  // E key
  B7: [ '2:4o2:312x' ],
  '#Cm': [ '4:15:26:46:34:1x' ],
  B: [ '2:144:34:22:1x' ],
  //A: [ 'o2:322:1ox' ],
  '#Gm': [ '4:14:14:16:46:34:1' ],
  //#Fm: [ '2:12:12:144:32:1'],
  E: [ 'oo12:32o' ],
  // F key
  C7: [ 'o13:423x' ],
  //Dm: [ '132oxx' ],
  //C: [ 'o1o23x' ],
  bB: [ '13:433:211' ],
  //Am: [ 'o12:32ox' ],
  Gm: [ '3:13:13:15:45:33:1' ],
  //F: [ '1123:431' ],
  // G key
  //B7 
  D7: [ '2:312oxx' ],
  //Em
  //D
  //C
  //Bm
  //Am
  //G
  //A key
  //E7
  //#Fm
  //E
  //D
  //#Cm
  //Bm
  //A
  //B key
  '#F7': [ '2:12:13:22:14:32:1' ],
  '#F': [ '2:12:13:244:32:1' ],
  //E
  '#Dm': [ '6:17:28:48:36:1x' ],
  //#Cm
  //B
};
export default {
  name: 'app',
  data(){
    return {
      chordName: '',
      chordString: '',
      gtpSetting: {
        hgap: 45,
        vgap: 45*0.618,
        pin: 4,
        init: 20,
        bg: '#eec',
        fg: '#369',
        ft: '18px Consolas bold',
        fts: '12px Consolas bold',
        w: 300,
        h: 280
      }
    }
  },
  methods:{
    getChordString: function(){
      if(chordTable[this.chordName]){
        this.chordString = chordTable[this.chordName][0];
      }else{
        this.chordString = null;
        //let objKeys = Object.keys( chordTable );
        //let lastKeyIndex = objKeys.length-1;
        //this.chordName = objKeys[lastKeyIndex];
        //this.chordString = chordTable[this.chordName][0];
      }
    }
  },
  components: {
    Canvas,
    Guitar
  },
}
</script>
<style>
@import "../node_modules/picnic/picnic.min.css";
#app{
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#app .input{
  margin-bottom: 2em;
}
</style>
