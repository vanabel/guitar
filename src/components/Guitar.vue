<script>
export default{
  name: 'Guitar',
  inject: ['provider'],
  props: {
    chordName: String,
    chordString: String,
    gtpSetting: Object
  },
  data: function(){
    return{
      temp: null
    }
  },
  render() {
    //Since the render function may run before the injection
    if(!this.provider.context)
      return null;
    var chordArray = stringToArray(this.chordString);
    var ctx = this.provider.context;
    let bg = this.gtpSetting.bg;
    let fg = this.gtpSetting.fg;
    let ft = this.gtpSetting.ft;
    let fts = this.gtpSetting.fts;
    let pin = this.gtpSetting.pin;
    let init = this.gtpSetting.init;
    let hgap = this.gtpSetting.hgap;
    let vgap = this.gtpSetting.vgap;
    let w = this.gtpSetting.w;
    let h = this.gtpSetting.h;
    ctx.fillStyle = bg;
    ctx.fillRect(0,0, w, h);
    // draw fretboard 
    // vertical line
    ctx.strokeStyle = fg;
    for(let i=0; i<=pin; i++){
      ctx.beginPath();
      ctx.moveTo(init+hgap*i, init);
      ctx.lineTo(init+hgap*i, init+5*vgap);
      ctx.lineWidth = (i===0?3:1);
      ctx.stroke();
      ctx.closePath();
    }
    // horizontal line
    for(let i=0; i<6; i++){
      ctx.beginPath();
      ctx.moveTo(init, init+vgap*i);
      ctx.lineTo(init+pin*hgap, init+vgap*i);
      ctx.lineWidth = 1+2/6*i;
      ctx.stroke();
      ctx.closePath();
    }
    if(chordArray!=''){
    // find the first fretboard number
      let chordFretArray = Array.from( chordArray, x => x === 'x' || x === 'o' ? 12: parseInt(x.charAt(0)) );
      let minFret = Math.min(...chordFretArray)-1;
      let minFretText = minFret+'ft';
      if( minFret > 1 ){
        chordArray = Array.from( chordArray, x => x === 'x' || x==='o' ? x: (parseInt(x.charAt(0))-minFret) + x.slice(1) ); 
        // mark the first fretboard
      ctx.beginPath();
      ctx.font= fts;
      ctx.textAlign='center';
      ctx.textBaseline='bottom';
      ctx.fillStyle= fg;
      ctx.fillText(minFretText, init, init);
      ctx.closePath();
      }

    // draw finger position background
    ctx.fillStyle= bg;
    for(let i=0; i<6; i++){
      ctx.beginPath();
      ctx.arc(init+xpos(chordArray[i])[0]-hgap/2, init+vgap*i, 8, 0, 2*Math.PI);
      ctx.fill();
      ctx.closePath();
    }

    // draw finger position
    ctx.fillStyle= fg;
    for(let i=0; i<6; i++){
      ctx.beginPath();
      ctx.font= ft;
      ctx.textAlign='end';
      ctx.textBaseline='middle';
      ctx.fillText(xpos(chordArray[i])[1], init+xpos(chordArray[i])[0]-hgap/2+8, init+vgap*i);
      ctx.closePath();
    }
    }
    /**
     * Calculate the finger position and text
     * Example input: 3
     * Example output: [3, 3]
     * Example input: 3:4
     * Example output: [3,4]
     */
    function xpos(str){
      // charMap: maps the number to circled number (utf8 encoded)
      var charMap = { '1': "\u2460", '2': "\u2461", '3': "\u2462", '4': "\u2463" };
      if(str === 'x' || str === 'o'){
        var pos=12;
        var finger=str;
      }else{
        pos=parseInt(str[0])*hgap;
        if(str.length >1){
          finger=charMap[str.charAt(2)];
        }else{
          finger=charMap[str.charAt(0)];
        }
      }
      return [pos, finger];
    }

    /**
     * Transform the input string to GTP array
     * Example input: '11233:41'
     * Example output: ['1', '1', '2', '3', '3:4', '1']; //F
     * Example input: 'o1o23x'
     * Example output: ['o', '1', 'o', '2', '3', 'x']; //C
     * Example input: '132oxx'
     * Example output: ['1', '3', '2', 'o', 'x', 'x']; //Dm
     */
    function stringToArray(str){
      let dataArr=[];
      //check the validity
      if(!str) return false;
      if(str.match(/:/g) && str.length!=str.match(/:/g).length*2+6){
        return false;
      }
      for(let i=0, j=0;i<str.length;i++){
        if(str.charAt(i+1)!=':'){
          dataArr[j]=str.charAt(i);
        }else{
          dataArr[j]=str.substring(i,i+3);
          i+=2;
        }
        j++;
      }
      return dataArr;
    }
    return null
  },
}
</script>
