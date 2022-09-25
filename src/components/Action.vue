<template>
  <div id="action"></div>
  <span id="add" v-if="addBtn">添加</span>
</template>

<script>
import {onMounted, ref} from "vue";

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Action",
  props:['id'],
  setup(props){
    // eslint-disable-next-line vue/no-mutating-props

    let addBtn = ref(false)

    onMounted(()=>{
      if(props.id == 1){
        console.log('a')
        addBtn.value = true
      }
    })
    return {addBtn}
  }

}
</script>

<style scoped>
  #action {
    position: relative;
    top: 10px;
    margin-left: 10px;
    width: 90px;
    height: 90px;
    background-color: pink;
    border: 1px solid #a79e7d;
    border-radius: 7px;
    display: inline-block;
  }
  #add {
    padding: 2px;
    position: absolute;
    top: 42%;
    left: 47.5%;
    /*background-color: white;*/
    background-color: transparent;
    border: 1px solid #2c3e50;
    border-radius: 5px;
    font-size: 16px;
    color: #1f2044;
  }
</style>