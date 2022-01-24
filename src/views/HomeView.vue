<template>
  <div class="home">  
    <Layout ref="layout" msg="Welcome to Your Vue.js + TypeScript App" />
  </div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import Layout from "@/components/Layout.vue"; // @ is an alias to /src
interface target { 
  pageY: number; 
  pageX: number; 
}
// interface TouchEvent {
//   changedTouches: Array<target>
// }
@Options({
  components: {
    Layout,
  },
})
export default class HomeView extends Vue {
  bgs = ['#673ab7','#3f51b5','#2196f3','#03a9f4','#00bcd4','#009688','#4caf50','#8bc34a','#ffeb3b','#ffc107']
  touch = {
    y: 0,
    x: 0,
    miles: 0,
    isLegal: true
  }
  index = 0
  touchstart(e: any) {
    this.touch.isLegal = true
    this.touch.y = e.changedTouches[0].pageY
    this.touch.x = e.changedTouches[0].pageX
    this.touch.miles = Date.now()
    // console.log(e);
  }
  touchmove(e: TouchEvent) {
    if(!this.touch.isLegal)return 
    // console.log(1);
    const { pageX, pageY } = e.changedTouches[0]
    const length3 = Math.sqrt(Math.pow(pageX-this.touch.x, 2) + Math.pow(pageY-this.touch.y, 2));
    const sinVal = (pageX - this.touch.x) / length3
    
    if(sinVal < -Math.SQRT1_2 || sinVal > Math.SQRT1_2) {
      console.log('步子大了',sinVal)
      this.touch.isLegal = false
    }
  }
  touchend(e: any) {
    const { pageX, pageY } = e.changedTouches[0]
    const distance = pageY - this.touch.y
    const miles = Date.now() - this.touch.miles
    if(this.touch.isLegal) {
      if(miles < 900 && distance < -50){
        this.next()
      }
    }
    console.log('distance:',distance,';miles:',miles);
  }
  next() {
    const {changeBg} = this.$refs.layout as any
    changeBg(this.bgs[(++this.index) % this.bgs.length])
    console.log('下一页了');
  }

  get style1() {
    return {
      background: '#4d9f0c'
    }
  }
}
</script>
<style lang="less" scoped>
.mask {
  // position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 9999;
  background: transparent;
  .next {
    position: fixed; 
    bottom: 0;
    z-index: 9999;
    width: 30px;;
    height: 30px;;
    background: transparent;
  }
}
</style>
