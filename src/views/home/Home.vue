<template>
  <div id="home">
    <nav-bar class="home-nav" >
      <div slot="center">购物街</div>
    </nav-bar>
    <home-swiper :banners="banners"></home-swiper>
    <recommend-view :recommends="recommend"></recommend-view>
    <feature-view></feature-view>
    <tab-control :titles="['流行','新款','精选']" class="tab-control"></tab-control>
    <goods-list :goods="goods['pop'].list"></goods-list>
  </div>
</template>

<script>
import NavBar from "@/components/common/navbar/NavBar";

import HomeSwiper from "@/views/home/childComps/HomeSwiper";
import RecommendView from "@/views/home/childComps/RecommendView";
import FeatureView from "@/views/home/childComps/FeatureView";

import TabControl from "@/components/content/tabControl/TabControl";
import GoodsList from "@/components/content/goods/GoodsList";

import {getHomeMultidata,getHomeGoods} from '@/network/home'

export default {
  name: "Home",
  components:{
    NavBar,
    HomeSwiper,
    RecommendView,
    FeatureView,
    TabControl,
    GoodsList
  },
  data(){
    return {
      banners:[],
      recommend:[],
      goods: {
        pop: {page:0 , list:[]},
        news: {page:0 , list:[]},
        sell: {page:0 , list:[]},
      },
      defaultGoods:[
        {
          show:{
            img:'https://s5.mogucdn.com/mlcdn/c45406/210217_4ej73ael4l5bjic5d9h3957lae93a_1080x1920.jpg_300x9999.v1c7E.70.webp',
          },
          title:'3秒学减灵百搭的丸子头',
          price:123.00,
          cfav:10
        },
        {
          show:{
            img:'https://s11.mogucdn.com/mlcdn/c45406/210216_391i9165h45g6fgf840676c5dcg8l_2160x2160.jpg_300x9999.v1c7E.70.webp',
          },
          title:'罗马线，高端打底衫',
          price:99.00,
          cfav:9
        },
        {
          show:{
            img:'https://s5.mogucdn.com/mlcdn/c45406/210211_5k6bk8helb4263g0ga62449i7dhdi_810x1080.jpg_300x9999.v1c7E.70.webp',
          },
          title:'在鱼儿直播间买的，一看就喜欢的得类型，上身效果特别好看，矮个子的妹子也可以穿。穿上它领证去',
          price:2300.00,
          cfav:10
        },
        {
          show:{
            img:'https://s5.mogucdn.com/mlcdn/c45406/210216_008f34feclaf447i37bh9697hf82a_1080x1440.jpg_300x9999.v1c7E.70.webp',
          },
          title:'初五迎财神 财神C位',
          price:80.00,
          cfav:3
        },
      ],
      defaultGoods2:[
        {
          show:{
            img:'https://s5.mogucdn.com/mlcdn/c45406/210215_5j3ad4hf32i631289ah932cbef6bi_607x1080.jpg_300x9999.v1c7E.70.webp',
          },
          title:'甜心推荐，直播间秒杀73元/2件，内衣质量很好，不是那种硬硬的料子，摸上去滑溜溜的，穿上应该会更舒服吧， 包装也很好， 价格很便宜买到这样的，物超所值了！很喜欢这个两个颜色，之前买过其他色的，手感很好，薄厚也适中，不太喜欢太厚的，这件刚刚好，码数也合适，整体挺满意赶上活动秒杀买的，价格很美丽！',
          price:123.00,
          cfav:10
        },
        {
          show:{
            img:'https://s5.mogucdn.com/mlcdn/c45406/200906_12h6k3bgblb51ichbchd99b22l370_1024x1922.jpg_300x9999.v1c7E.70.webp',
          },
          title:'不会画眉的速看',
          price:99.00,
          cfav:9
        },
        {
          show:{
            img:'https://s5.mogucdn.com/mlcdn/c45406/200709_3ifhd7bad2b3b1f9bh37c9k3i622k_1080x1908.png_999x999.v1c0.100.webp',
          },
          title:'分享神仙手链🌈软糯绿豆糕，和田玉晴水料！一物两用款！色泽甜美！',
          price:2300.00,
          cfav:10
        },
        {
          show:{
            img:'https://s5.mogucdn.com/mlcdn/c45406/210216_008f34feclaf447i37bh9697hf82a_1080x1440.jpg_300x9999.v1c7E.70.webp',
          },
          title:'初五迎财神 财神C位',
          price:80.00,
          cfav:3
        },
      ]
    }
  },
  created() {
    this.getHomeMultidata()

    for (let type in this.goods){
      this.getHomeGoods(type)
    }
  },
  methods:{
    //请求轮播和推荐数据
    getHomeMultidata(){
      getHomeMultidata().then(res=>{
        this.banners = res.data.banner.list
        this.recommend = res.data.recommend.list
      })
    },
    //请求商品数据
    getHomeGoods(type){
      const page = this.goods[type].page + 1
      getHomeGoods(type , page).then(res=>{
        //接口变更无数据了
        // this.goods[type].list.push(...res.data.list)
        //模拟数据
        if (parseInt(Math.random()*2+1) == 1){
          this.goods[type].list.push(...this.defaultGoods)
        }else{
          this.goods[type].list.push(...this.defaultGoods2)
        }
        this.goods[type].page++

      })
    }
  }
}
</script>

<style scoped>
  #home{
    padding-top: 44px;
  }
  .home-nav{
    background-color: var(--color-tint);
    color: white;

    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    z-index: 9;
  }

  .tab-control{
    position: sticky;
    top: 44px;
  }
</style>
