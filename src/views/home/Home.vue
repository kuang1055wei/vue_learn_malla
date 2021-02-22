<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
    <tab-control
      :titles="['流行','新款','精选']"
      @tabClick="tabClick"
      ref="tabControl1"
      class="tab-control"
      v-show="isTabFixed"
    ></tab-control>
    <scroll class="content"
            ref="scroll"
            :probe-type="3"
            :pullUpLoad="true"
            @scroll="contentScroll"
            @pullingUp="loadMore"
    >
      <home-swiper :banners="banners" @swiperImageLoad="swiperImageLoad"></home-swiper>
      <recommend-view :recommends="recommend"></recommend-view>
      <feature-view></feature-view>
      <tab-control
        :titles="['流行','新款','精选']"
        @tabClick="tabClick"
        ref="tabControl2"
      ></tab-control>
      <goods-list :goods="showGoods"></goods-list>
    </scroll>
    <back-top @click.native="backClick" v-show="isShowBackTop"></back-top>
  </div>
</template>

<script>
import NavBar from "@/components/common/navbar/NavBar";

import HomeSwiper from "@/views/home/childComps/HomeSwiper";
import RecommendView from "@/views/home/childComps/RecommendView";
import FeatureView from "@/views/home/childComps/FeatureView";

import TabControl from "@/components/content/tabControl/TabControl";
import GoodsList from "@/components/content/goods/GoodsList";
import BackTop from "@/components/content/backTop/BackTop";

import {getHomeMultidata, getHomeGoods} from '@/network/home'

import Scroll from "@/components/common/scroll/Scroll";

import {debounce} from '@/common/untils'


export default {
  name: "Home",
  components: {
    NavBar,
    HomeSwiper,
    RecommendView,
    FeatureView,
    TabControl,
    GoodsList,
    Scroll,
    BackTop
  },
  data() {
    return {
      banners: [],
      recommend: [],
      goods: {
        pop: {page: 0, list: []},
        new: {page: 0, list: []},
        sell: {page: 0, list: []},
      },
      currentType: 'pop',
      isShowBackTop: false,
      tabOffsetTop:0,//tab的距离
      isTabFixed:false,
      saveY:0,
      defaultGoods: [
        {
          id:1,
          show: {
            img: 'https://s5.mogucdn.com/mlcdn/c45406/210217_4ej73ael4l5bjic5d9h3957lae93a_1080x1920.jpg_300x9999.v1c7E.70.webp',
          },
          title: '3秒学减灵百搭的丸子头',
          price: 123.00,
          cfav: 10
        },
        {
          id:2,
          show: {
            img: 'https://s11.mogucdn.com/mlcdn/c45406/210216_391i9165h45g6fgf840676c5dcg8l_2160x2160.jpg_300x9999.v1c7E.70.webp',
          },
          title: '罗马线，高端打底衫',
          price: 99.00,
          cfav: 9
        },
        {
          id:3,
          show: {
            img: 'https://s5.mogucdn.com/mlcdn/c45406/210211_5k6bk8helb4263g0ga62449i7dhdi_810x1080.jpg_300x9999.v1c7E.70.webp',
          },
          title: '在鱼儿直播间买的，一看就喜欢的得类型，上身效果特别好看，矮个子的妹子也可以穿。穿上它领证去',
          price: 2300.00,
          cfav: 10
        },
        {
          id:4,
          show: {
            img: 'https://s5.mogucdn.com/mlcdn/c45406/210216_008f34feclaf447i37bh9697hf82a_1080x1440.jpg_300x9999.v1c7E.70.webp',
          },
          title: '初五迎财神 财神C位',
          price: 80.00,
          cfav: 3
        },
        {
          id:5,
          show: {
            img: 'https://s5.mogucdn.com/mlcdn/c45406/200709_3ifhd7bad2b3b1f9bh37c9k3i622k_1080x1908.png_999x999.v1c0.100.webp',
          },
          title: '分享神仙手链🌈软糯绿豆糕，和田玉晴水料！一物两用款！色泽甜美！',
          price: 2300.00,
          cfav: 10
        },
        {
          id:6,
          show: {
            img: 'https://s5.mogucdn.com/mlcdn/c45406/210216_008f34feclaf447i37bh9697hf82a_1080x1440.jpg_300x9999.v1c7E.70.webp',
          },
          title: '初五迎财神 财神C位',
          price: 80.00,
          cfav: 3
        },
        {
          id:7,
          show: {
            img: 'https://s5.mogucdn.com/mlcdn/c45406/200826_85kb501f94aclg731dc3b3glkfca0_1080x1906.png_999x999.v1c0.100.webp',
          },
          title: '2020最新款，云朵，来啦！太好看了吧！这么好看怎么办😘😘喜欢的小姐姐扣666今晚9点送福利啦9点9点！',
          price: 2300.00,
          cfav: 10
        },
        {
          id:8,
          show: {
            img: 'https://s5.mogucdn.com/mlcdn/c45406/210216_008f34feclaf447i37bh9697hf82a_1080x1440.jpg_300x9999.v1c7E.70.webp',
          },
          title: '初五迎财神 财神C位',
          price: 80.00,
          cfav: 3
        },
        {
          id:9,
          show: {
            img: 'https://s5.mogucdn.com/mlcdn/c45406/200826_85kb501f94aclg731dc3b3glkfca0_1080x1906.png_999x999.v1c0.100.webp',
          },
          title: '2020最新款，云朵，来啦！太好看了吧！这么好看怎么办😘😘喜欢的小姐姐扣666今晚9点送福利啦9点9点！',
          price: 2300.00,
          cfav: 10
        },
        {
          id:10,
          show: {
            img: 'https://s5.mogucdn.com/mlcdn/c45406/210216_008f34feclaf447i37bh9697hf82a_1080x1440.jpg_300x9999.v1c7E.70.webp',
          },
          title: '初五迎财神 财神C位',
          price: 80.00,
          cfav: 3
        },
        {
          id:11,
          show: {
            img: 'https://s5.mogucdn.com/mlcdn/c45406/200826_85kb501f94aclg731dc3b3glkfca0_1080x1906.png_999x999.v1c0.100.webp',
          },
          title: '2020最新款，云朵，来啦！太好看了吧！这么好看怎么办😘😘喜欢的小姐姐扣666今晚9点送福利啦9点9点！',
          price: 2300.00,
          cfav: 10
        },{
          id:12,
          show: {
            img: 'https://s5.mogucdn.com/mlcdn/c45406/210216_008f34feclaf447i37bh9697hf82a_1080x1440.jpg_300x9999.v1c7E.70.webp',
          },
          title: '初五迎财神 财神C位',
          price: 80.00,
          cfav: 3
        },
        {
          id:13,
          show: {
            img: 'https://s5.mogucdn.com/mlcdn/c45406/200826_85kb501f94aclg731dc3b3glkfca0_1080x1906.png_999x999.v1c0.100.webp',
          },
          title: '2020最新款，云朵，来啦！太好看了吧！这么好看怎么办😘😘喜欢的小姐姐扣666今晚9点送福利啦9点9点！',
          price: 2300.00,
          cfav: 10
        },{
          id:14,
          show: {
            img: 'https://s5.mogucdn.com/mlcdn/c45406/210216_008f34feclaf447i37bh9697hf82a_1080x1440.jpg_300x9999.v1c7E.70.webp',
          },
          title: '初五迎财神 财神C位',
          price: 80.00,
          cfav: 3
        },
        {
          id:15,
          show: {
            img: 'https://s5.mogucdn.com/mlcdn/c45406/200826_85kb501f94aclg731dc3b3glkfca0_1080x1906.png_999x999.v1c0.100.webp',
          },
          title: '2020最新款，云朵，来啦！太好看了吧！这么好看怎么办😘😘喜欢的小姐姐扣666今晚9点送福利啦9点9点！',
          price: 2300.00,
          cfav: 10
        },{
          id:16,
          show: {
            img: 'https://s5.mogucdn.com/mlcdn/c45406/210216_008f34feclaf447i37bh9697hf82a_1080x1440.jpg_300x9999.v1c7E.70.webp',
          },
          title: '初五迎财神 财神C位',
          price: 80.00,
          cfav: 3
        },
        {
          id:17,
          show: {
            img: 'https://s5.mogucdn.com/mlcdn/c45406/200826_85kb501f94aclg731dc3b3glkfca0_1080x1906.png_999x999.v1c0.100.webp',
          },
          title: '2020最新款，云朵，来啦！太好看了吧！这么好看怎么办😘😘喜欢的小姐姐扣666今晚9点送福利啦9点9点！',
          price: 2300.00,
          cfav: 10
        },
      ],
      defaultGoods2: [
        {
          id:1,
          show: {
            img: 'https://s5.mogucdn.com/mlcdn/c45406/210215_5j3ad4hf32i631289ah932cbef6bi_607x1080.jpg_300x9999.v1c7E.70.webp',
          },
          title: '甜心推荐，直播间秒杀73元/2件，内衣质量很好，不是那种硬硬的料子，摸上去滑溜溜的，穿上应该会更舒服吧， 包装也很好， 价格很便宜买到这样的，物超所值了！很喜欢这个两个颜色，之前买过其他色的，手感很好，薄厚也适中，不太喜欢太厚的，这件刚刚好，码数也合适，整体挺满意赶上活动秒杀买的，价格很美丽！',
          price: 123.00,
          cfav: 10
        },
        {
          id:2,
          show: {
            img: 'https://s5.mogucdn.com/mlcdn/c45406/200906_12h6k3bgblb51ichbchd99b22l370_1024x1922.jpg_300x9999.v1c7E.70.webp',
          },
          title: '不会画眉的速看',
          price: 99.00,
          cfav: 9
        },
        {
          id:3,
          show: {
            img: 'https://s5.mogucdn.com/mlcdn/c45406/200709_3ifhd7bad2b3b1f9bh37c9k3i622k_1080x1908.png_999x999.v1c0.100.webp',
          },
          title: '分享神仙手链🌈软糯绿豆糕，和田玉晴水料！一物两用款！色泽甜美！',
          price: 2300.00,
          cfav: 10
        },
        {
          id:4,
          show: {
            img: 'https://s5.mogucdn.com/mlcdn/c45406/210216_008f34feclaf447i37bh9697hf82a_1080x1440.jpg_300x9999.v1c7E.70.webp',
          },
          title: '初五迎财神 财神C位',
          price: 80.00,
          cfav: 3
        },
        {
          id:5,
          show: {
            img: 'https://s5.mogucdn.com/mlcdn/c45406/210211_5k6bk8helb4263g0ga62449i7dhdi_810x1080.jpg_300x9999.v1c7E.70.webp',
          },
          title: '在鱼儿直播间买的，一看就喜欢的得类型，上身效果特别好看，矮个子的妹子也可以穿。穿上它领证去',
          price: 2300.00,
          cfav: 10
        },
        {
          id:6,
          show: {
            img: 'https://s5.mogucdn.com/mlcdn/c45406/210216_008f34feclaf447i37bh9697hf82a_1080x1440.jpg_300x9999.v1c7E.70.webp',
          },
          title: '初五迎财神 财神C位',
          price: 80.00,
          cfav: 3
        },
      ]
    }
  },
  created() {
    this.getHomeMultidata()

    for (let type in this.goods) {
      this.getHomeGoods(type)
    }
  },
  activated() {
    this.$refs.scroll.scrollTo(0,this.saveY)
  },
  deactivated() {
    this.saveY = this.$refs.scroll.getScrollY()
  },
  mounted() {

    //使用防抖函数在图片加载完成后重新计算scroll的高度
    const refresh = debounce(this.$refs.scroll.refresh , 500)

    //监听商品图片加载完成
    this.$bus.$on('itemImageLoad' , ()=>{
      refresh();
      // console.log('itemImageLoad')
      // this.$refs.scroll.refresh()
    })

  },
  computed: {
    showGoods() {
      return this.goods[this.currentType].list
    },

  },
  methods: {

    backClick() {
      this.$refs.scroll.scrollTo(0, 0, 500)
    },


    //返回按钮显示
    contentScroll(position) {
      //判断backTop显示按钮
      let {x, y} = position
      this.isShowBackTop = y < -1000

      //决定tabControl是否吸顶
      this.isTabFixed = (-y) >= this.tabOffsetTop

      // console.log(-y,this.tabOffsetTop)
    },

    tabClick(index) {
      switch (index) {
        case 0:
          this.currentType = 'pop'
          break
        case 1:
          this.currentType = 'new'
          break
        case 2:
          this.currentType = 'sell'
          break
      }

      this.$refs.tabControl1.currentIndex = index
      this.$refs.tabControl2.currentIndex = index
      // setTimeout(()=>{
      //   this.$refs.scroll.refresh()
      //
      // },1000)

    },

    //请求轮播和推荐数据
    getHomeMultidata() {
      getHomeMultidata().then(res => {
        this.banners = res.data.banner.list
        this.recommend = res.data.recommend.list
      })
    },
    //请求商品数据
    getHomeGoods(type) {
      const page = this.goods[type].page + 1
      getHomeGoods(type, page).then(res => {
        //接口变更无数据了
        // this.goods[type].list.push(...res.data.list)
        //模拟数据
        if (type == 'pop') {
          this.goods[type].list.push(...this.defaultGoods)
        }
        if (type == 'new') {
          this.goods[type].list.push(...this.defaultGoods2)
        }
        if (type == 'sell') {
          this.goods[type].list.push(...this.defaultGoods)
          this.goods[type].list.reverse()
        }
        this.goods[type].page++

        this.$refs.scroll.finishPullUp()

      })
    },
    //加载更多
    loadMore(){
      this.getHomeGoods(this.currentType)
    },
    swiperImageLoad(){
      //获取tabControl的offsetTop
      this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop
    }

    // //防抖
    // debounce(func , delay){
    //   let timer = null
    //   console.log(timer)
    //   return function (...args) {
    //     if (timer) clearTimeout(timer)
    //     timer = setTimeout(()=>{
    //       func.apply(this,args)
    //     } , delay)
    //   }
    // },
    // //节流
    // throttle(){
    //
    // }
  },

}
</script>

<style scoped>
#home {
  /*padding-top: 44px;*/
  height: 100vh;
}

.home-nav {
  background-color: var(--color-tint);
  color: white;

  /*position: fixed;*/
  /*left: 0;*/
  /*right: 0;*/
  /*top: 0;*/
  /*z-index: 9;*/
}

/*使用了better-scroll后吸顶效果无效*/
/*.tab-control {*/
/*  position: sticky;*/
/*  top: 44px;*/
/*  z-index: 9;*/
/*}*/

.tab-control {
  position: relative;
  z-index: 9;
}

/*.content {*/
/*  overflow: hidden;*/

/*  position: absolute;*/
/*  top: 44px;*/
/*  bottom: 49px;*/
/*  left: 0;*/
/*  right: 0;*/
/*}*/

.content{
  height:calc(100% - 44px);
  overflow: hidden;
  /*margin-top: 44px;*/
    /*height: 300px;*/
  /*overflow: hidden;*/
}
</style>
