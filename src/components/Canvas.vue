<template>
  <div class='canvas-wrap'>
    <canvas ref='canvas'></canvas>
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: 'Canvas',
  props: {
    gtpSetting: Object
  },
  data(){
    return {
      provider: {
        context: null
      },
      temp: null,
    }
  },
  provide(){
    return{
      provider: this.provider
    }
  },
  mounted() {
    this.provider.context = this.$refs['canvas'].getContext('2d');
    let pin = this.gtpSetting.pin;
    let init = this.gtpSetting.init;
    let vgap = this.gtpSetting.vgap;
    let hgap = this.gtpSetting.hgap;
    this.gtpSetting.w = pin*hgap+init*2;
    this.gtpSetting.h = 5*vgap+init*2;
    this.$refs['canvas'].width = this.gtpSetting.w;
    this.$refs['canvas'].height = this.gtpSetting.h;
  }
}
</script>
<style>
.gtp{
  border: 3px solid #eee;
}
.canvas-wrap{
  margin: 0 1em 5em 0;
  /* -webkit-transform:rotate(90deg); */
}
</style>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<!-- <style scoped>
#canvas{
  -webkit-transform:rotate(90deg);
  -moz-transform:rotate(90deg);
  -o-transform:rotate(90deg);
  -ms-transform:rotate(90deg);
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style> -->
