<template>
  <div id="app">
    <TransitionGroup name="slide-fade">
      <p v-if="show">/</p>
      <p v-if="show2" id="p2">/</p>
    </TransitionGroup>
    <div class="sliders">
      <slider  v-for="(id,index) in 7" :key=index :Id="id"></slider>
    </div>
    <div class="sliders" id="group2">
      <slider  v-for="(id,index) in 7 " :key=index :Id=id+7></slider>
    </div>
    <Controller></Controller>
    <ActionCenter></ActionCenter>
  </div>
</template>

<style>
*{
  background-color: #1F2044;
}
#app {
  position: relative;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.sliders {
  /*display: inline-block;*/
  position: absolute;
  /*padding-left: 10%;*/
  /*left: -5%;*/
  width: 250px;
  background-color: rgba(10,10,0,.2);
  border: 1px solid grey;
  border-radius: 10px;
  z-index: -1;
}
#group2 {
  left: 280px;
}
.slide-fade-enter-active {
  transition: all 1.7s cubic-bezier(0.5, 0.8, 0.8, 1);
}

.slide-fade-leave-active {
  transition: all 6.8s cubic-bezier(0.5, 0.8, 0.8, 1);
}

.slide-fade-enter-from{
  opacity: 1;
}
.slide-fade-leave-to {
  transform: translate(800px,-2200px);
  opacity: 0;
}
p{
  position: fixed;
  bottom: -20px;
  left: -20px;
  z-index: -3;
  font-size: 50px;
  color: orange;
}
#p2{
  bottom: -150px;
  left: 30px;
  color: peachpuff;
}
</style>
<script>
import Controller from "@/components/Controller";
import Slider from "@/components/Slider"
import ActionCenter from "@/components/ActionCenter";
export default {
  // eslint-disable-next-line vue/no-unused-components
  components: {Controller,Slider,ActionCenter},
  data(){
    return {
      startIndex:0,
      numberOfServo:14,
      show:false,
      show2:false,
    }
  },
  mounted() {
    window.setInterval(()=>{
      this.show = !this.show
    },2000)
    window.setInterval(()=>{
      this.show2 = !this.show2
    },1500)
    this.$store.angle = new Array();
    for(let i=0;i<14;i++){
      this.$store.angle[i]=90
    }
  }
}
</script>