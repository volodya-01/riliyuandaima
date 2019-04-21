<template>
  <div class="hello">
    <div @click="outparents()" style="width:300px;height:60px;background:red">
                <span @click.stop="inson()" style="display:inline-block;width:90px;height:30px;background:#fff">按钮1</span>
                <span>按钮2</span>
                <input type="text" @click.stop="stopstop">
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      msg: "Welcome to Your Vue.js App"
    };
  },
methods:{
  outparents(){
    alert("我是outparents()")
  },
  inson(){
    alert("我是inson()")
  },
  stopstop(){
   /*  console.log('') */
  }
}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
