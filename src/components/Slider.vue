<template>
  <div id="outer" ref="outer">
    <div id="inner">
      <div id="innerAgent" style="left: 0;" ref="innerAgent">
        <div id="block" ref="block" @mousedown="move" @mouseup="remove">
          <div id="line"></div>
          <div id="currentAngle">{{this.angle}}</div>
        </div>
      </div>
      <div id="mark">
        <div id="left">0</div>
        <div id="right">180</div>
      </div>
    </div>
  </div>
</template>

<script>

const FACTOR = 140/180;
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Slider",
  data(){
    return {
      sliderVal:0,
      offset:0,
    }
  },
  props:['Id'],
  computed:{
    angle:{
      get(){
        return Math.round(this.sliderVal*1.265)
      },
      set(val){
        this.sliderVal = Math.round(val/1.265)
      }
    }
  },
  methods: {
    move(){
      // eslint-disable-next-line no-unused-vars
      this.$refs.outer.addEventListener('mousemove',this.getPosition,false)
    },
    remove: function () {
      this.$refs.outer.removeEventListener('mousemove',this.getPosition,false)
    },
    getPosition(e){
      this.sliderVal = e.clientX-39+this.offset;
      if(this.angle>180){
        this.angle = 180
      }
      else if(this.angle<0){
        this.angle=0
      }
      this.$refs.block.style.left = `${this.sliderVal}px`
      if(this.angle>=90){
        this.$refs.innerAgent.style.width= `${(this.angle-90)/0.9}%`
      }else{
        this.$refs.innerAgent.style.width= `${(90-this.angle)/0.9}%`
      }
      this.$store.angle[this.Id-1] = this.angle
    }
  },
  mounted() {
    this.$refs.block.style.left=`${90*FACTOR}px`
    if(this.Id>7){
      // eslint-disable-next-line vue/no-mutating-props
      this.offset = -280
    }
    this.angle=90
  }
}
</script>

<style scoped>
  #line {
    position: absolute;
    top: -10px;
    left: 8px;
    transform: translateX(-50%);
    width: 1px;
    height: 10px;
    background-color: white;
  }
  #currentAngle{
    position: absolute;
    top: -20px;
    left: 5px;
    ransform: translateX(-50%);
    font-size: 8px;
    color: white;
  }
  #outer{
    position: relative;
    width: 180px;
    height: 17px;
    margin: 30px 10px;
    /*background-color: #282928;*/
    border: 1px solid #252C3A;
    border-radius: 6px;
    color: #A79E7D;
  }
  #inner {
    position: relative;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
    width: 80%;
    height: 5px;
    background-color: #000000;
  }
  #block {
    position: relative;
    width: 15px;
    height: 15px;
    transform: translate(-50%,0%);
    background-color: #A79E7D;
    border-radius: 20%;
  }
  #innerAgent {
    position: relative;
    left: 50%;
    height: inherit;
    width: 0%;
    background-color: #7D755D;
  }
  #mark {
    position: absolute;
    top: 11px;
    width: 100%;
    height: 7px;
    border: 1px solid #B5B2A0 ;
    border-radius: 2px;
    border-bottom: 1px solid transparent;
    z-index: -1;
  }
  #mark * {
    display: inline-block;
    width: fit-content;
    position: absolute;
    top: 5px;
    font-size: 10px;
  }
  #mark #left {
    left: -3px;
  }
  #mark #right {
    right: -8px;
  }
</style>