
<template>
  <div class="hello"> 
        <audio
        ref="audio"
        loop
        autoplay
        style="display:none"
        controls
        src="https://cdn.popochiu.com/music/bgm.mp3">
            Your browser does not support the
            <code>audio</code> element.
         </audio>  
        <div class="mask" @touchstart="touchstart"
    @touchmove="touchmove"
    @touchend="touchend">
            <div class="bg" :style="{background: bg}">
                <div class="music" :class="{ 'pause': !isPlay, }" @click="stopOrPause" >
                    <!-- <svg t="1643014676337" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="2229" width="2rem" height="2rem"><path d="M897.714 174.5l0 562.5q0 25.111-17.077 44.698t-43.192 30.384-51.982 16.072-48.466 5.273-48.466-5.273-51.982-16.072-43.192-30.384-17.077-44.698 17.077-44.698 43.192-30.384 51.982-16.072 48.466-5.273q52.734 0 96.428 19.587l0-269.698-385.714 119.03 0 356.083q0 25.111-17.077 44.698t-43.193 30.384-51.982 16.072-48.466 5.273-48.466-5.273-51.982-16.072-43.193-30.384-17.077-44.698 17.077-44.698 43.193-30.384 51.982-16.072 48.466-5.273q52.734 0 96.428 19.587l0-485.658q0-15.569 9.542-28.376t24.609-17.829l417.858-128.572q6.026-2.009 14.063-2.009 20.089 0 34.151 14.063t14.063 34.151z" p-id="2230" fill="#707070"></path></svg> -->
                    <svg t="1643015208167" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="7487" width="36" height="36"><path d="M855.733157 63.83998 168.26582 63.83998c-57.672514 0-104.425328 46.752814-104.425328 104.426351l0 687.46836c0 57.672514 46.752814 104.425328 104.425328 104.425328L855.733157 960.16002c57.672514 0 104.425328-46.752814 104.425328-104.425328L960.158485 168.266331C960.158485 110.592794 913.406694 63.83998 855.733157 63.83998zM724.777593 539.67327c-5.284353 13.393032-17.958 33.710776-31.210839 46.046732-14.598486 13.586436-21.436218 18.960841-35.677571 21.563108-7.648191 1.39886 7.611352-17.064654 12.644995-22.575158 19.836791-21.795399 28.535917-57.634652 25.994024-86.729294-4.040013-45.862537-58.316174-108.706841-96.937796-116.975155l-63.732533 259.038708c-15.70468 63.832817-24.083511 89.279373-51.118238 113.140827-65.715701 58.007135-163.949003 63.996546-191.106526-10.79895-5.092995-14.026458-10.534937-40.192398-0.165776-61.47205 31.800263-65.397453 131.969661-90.960665 199.63476-37.420261L595.721285 226.397287l32.317033 7.951089C613.600491 349.916485 781.192464 396.384819 724.777593 539.67327z" p-id="7488" fill="#8a8a8a"></path></svg>
                </div>
            </div> 
        </div>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";

@Options({
  props: {
    msg: String,
  },
})
export default class Layout extends Vue {
  msg!: string;
  isPlay = false
  show = false;
  bg = `radial-gradient(ellipse at top, #e66465, transparent),
            radial-gradient(ellipse at bottom, #4d9f0c, transparent);`;
  toggle() {
    this.show = !this.show;
  }
  play() {
      this.isPlay = true;
      (this.$refs.audio as any).play()
  }

 stopOrPause() {
    this.isPlay = !this.isPlay;
    this.isPlay ? (this.$refs.audio as any).play(): (this.$refs.audio as any).pause()
 }

  changeBg(bg = `radial-gradient(ellipse at top, #e66465, transparent),
            radial-gradient(ellipse at bottom, #4d9f0c, transparent);`) {
    this.bg = bg
  }

  bgs = ['linear-gradient(#e66465, #9198e5);','linear-gradient(0.25turn, #3f87a6, #ebf8e1, #f69d3c);','#673ab7','#3f51b5','#2196f3','#03a9f4','#00bcd4','#009688','#4caf50','#8bc34a','#ffeb3b','#ffc107','url(https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fpic.51yuansu.com%2Fpic3%2Fcover%2F02%2F57%2F60%2F59fb818b29d53_610.jpg&refer=http%3A%2F%2Fpic.51yuansu.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1645608412&t=9785003dffb9c2bb927d6baf5de00527)']
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
    this.changeBg(this.bgs[(this.index++) % this.bgs.length])
    console.log('下一页了');
  }

  get style1() {
    return {
      background: '#4d9f0c'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
.hello {
    .mask {
  position: fixed;
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
  .bg {
    transition: all .55s linear .35s;
    position: relative;
    background: 
    radial-gradient(ellipse at top, #e66465, transparent),
            radial-gradient(ellipse at bottom, #4d9f0c, transparent);
    width: 100%;
    height: 100vh;
    .music {
        position: absolute;
        top: 16px;; 
        right: 16px;
        font-size: 12px;
        animation: infiniteRotate 3s linear infinite;
    }
    .pause {
        animation-play-state: paused;
    }
  }
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
@keyframes infiniteRotate {
    0% {
        transform: rotate(0);
    }
    100% {
        transform: rotate(360deg);
    }
}
</style>
