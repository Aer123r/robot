<template>
  <div>
    <div id="timeInterval">
      <div id="timeSlogan">时间间隔</div>
    </div>
    <div id="actionCenter">
      <Action v-for="(id,index) in 1" :key="index" :id="id"></Action>
    </div>
    <div id="playBtn">PLAY</div>
  </div>

</template>

<script>
import Action from "@/components/Action";
export default {
  name: "ActionCenter",
  components: {Action}
}
</script>

<style scoped>
  #timeSlogan {
    width: 100px;
    height: 30px;
    border-radius: 3px 0 0 3px;
    background-color: #E0B412;
    color: #ffffff;
  }
  #timeInterval {
    position: absolute;
    top: 400px;
    width: 100px;
    height: 120px;
    left: 10px;
    border-radius: 10px;
    color: peachpuff;
  }
  #actionCenter {
    position: absolute;
    left: 110px;
    top: 400px;
    width: 540px;
    height: 120px;
    background-color: rgba(0,0,0,.3);
    border: 3px solid #E0B412;
    border-radius:0 10px 10px 10px;
    color: peachpuff;
    overflow: scroll;
  }
  #playBtn {
    position: absolute;
    left: 660px;
    top: 400px;
    padding: 3px;
    background-color: #E0B412;
    border: 1px solid #7d755d;
    border-radius: 4px;
    color: white;
  }
  #playBtn:hover {
    cursor: grab;
  }
</style>