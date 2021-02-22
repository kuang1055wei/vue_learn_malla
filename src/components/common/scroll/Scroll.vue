<template>
  <div class="wrapper" ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
import BScroll from 'better-scroll'

export default {
  name: "Scroll",
  props:{
    //是否监听滚动
    probeType:{
      type:Number,
      default:0
    },
    pullUpLoad:{
      type:Boolean,
      default: false
    }
  },
  data(){
    return {
      scroll:null
    }
  },
  mounted() {
    this.scroll = new BScroll(this.$refs.wrapper , {
      probeType: this.probeType,
      pullUpLoad:this.pullUpLoad,
      click:true
    })

    if (this.probeType == 2 || this.probeType==3) {
      this.scroll.on('scroll', (position) => {
        // console.log(position)
        this.$emit('scroll', position)
      })
    }

    if (this.pullUpLoad) {
      //上拉事件监听
      this.scroll.on('pullingUp', () => {
        this.$emit('pullingUp')
        // setTimeout(()=>{
        //   this.scroll.finishPullUp()
        // },2000)
      })

    }
  },
  methods:{
     scrollTo(x,y,time=300){
       this.scroll.scrollTo(x,y,time)
     },
    finishPullUp(){
      this.scroll.finishPullUp()
    },
    refresh(){
      this.scroll.refresh()
    },
    getScrollY(){
       return this.scroll.y
    }
  }
}
</script>

<style scoped>

</style>
