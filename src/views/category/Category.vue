<template>
  <div class="wrapper" ref="mywrapper">
    <ul>
      <li>1呵呵呵呵好</li>
      <li>2呵呵呵呵好</li>
      <li>3呵呵呵呵好</li>
      <li>4呵呵呵呵好</li>
      <li>5呵呵呵呵好</li>
      <li>6呵呵呵呵好</li>
      <li>7呵呵呵呵好</li>
      <li>8呵呵呵呵好</li>
      <li>9呵呵呵呵好</li>
      <li>10呵呵呵呵好</li>
      <li>11呵呵呵呵好</li>
      <li>12呵呵呵呵好</li>
      <li>13呵呵呵呵好</li>
      <li>14呵呵呵呵好</li>
      <li>15呵呵呵呵好</li>
      <li>16呵呵呵呵好</li>
      <li>17呵呵呵呵好</li>
      <li>18呵呵呵呵好</li>
      <li>19呵呵呵呵好</li>
      <li>20呵呵呵呵好</li>
      <li>21呵呵呵呵好</li>
      <li>22呵呵呵呵好</li>
      <li>23呵呵呵呵好</li>
      <li>24呵呵呵呵好</li>
      <li>25呵呵呵呵好</li>
      <li>26呵呵呵呵好</li>
      <li>27呵呵呵呵好</li>
      <li>28呵呵呵呵好</li>
      <li>29呵呵呵呵好</li>
      <li>30呵呵呵呵好</li>
      <li>31呵呵呵呵好</li>
      <li>32呵呵呵呵好</li>
      <li>33呵呵呵呵好</li>
      <li>34呵呵呵呵好</li>
      <li>35呵呵呵呵好</li>
      <li>36呵呵呵呵好</li>
      <li>37呵呵呵呵好</li>
      <li>38呵呵呵呵好</li>
      <li>39呵呵呵呵好</li>
      <li>40呵呵呵呵好</li>
      <li>41呵呵呵呵好</li>
      <li>42呵呵呵呵好</li>
      <li>43呵呵呵呵好</li>
      <li>44呵呵呵呵好</li>
      <li>45呵呵呵呵好</li>
      <li>46呵呵呵呵好</li>
      <li>47呵呵呵呵好</li>
      <li>48呵呵呵呵好</li>
      <li>49呵呵呵呵好</li>
      <li>50呵呵呵呵好</li>
      <li>51呵呵呵呵好</li>
      <li>52呵呵呵呵好</li>
      <li>53呵呵呵呵好</li>
      <li>54呵呵呵呵好</li>
      <li>55呵呵呵呵好</li>
      <li>56呵呵呵呵好</li>
      <li>57呵呵呵呵好</li>
      <li>58呵呵呵呵好</li>
      <li>59呵呵呵呵好</li>
      <li>60呵呵呵呵好</li>
      <li>61呵呵呵呵好</li>
      <li>62呵呵呵呵好</li>
      <li>63呵呵呵呵好</li>
      <li>64呵呵呵呵好</li>
      <li>65呵呵呵呵好</li>
      <li>66呵呵呵呵好</li>
      <li>67呵呵呵呵好</li>
      <li>68呵呵呵呵好</li>
      <li>69呵呵呵呵好</li>
      <li>70呵呵呵呵好</li>
      <li>71呵呵呵呵好</li>
      <li>72呵呵呵呵好</li>
      <li>73呵呵呵呵好</li>
      <li>74呵呵呵呵好</li>
      <li>75呵呵呵呵好</li>
      <li>76呵呵呵呵好</li>
      <li>77呵呵呵呵好</li>
      <li>78呵呵呵呵好</li>
      <li>79呵呵呵呵好</li>
      <li>80呵呵呵呵好</li>
      <li>81呵呵呵呵好</li>
      <li>82呵呵呵呵好</li>
      <li>83呵呵呵呵好</li>
      <li>84呵呵呵呵好</li>
      <li>85呵呵呵呵好</li>
      <li>86呵呵呵呵好</li>
      <li>87呵呵呵呵好</li>
      <li>88呵呵呵呵好</li>
      <li>89呵呵呵呵好</li>
      <li>90呵呵呵呵好</li>
      <li>91呵呵呵呵好</li>
      <li>92呵呵呵呵好</li>
      <li>93呵呵呵呵好</li>
      <li>94呵呵呵呵好</li>
      <li>95呵呵呵呵好</li>
      <li>96呵呵呵呵好</li>
      <li>97呵呵呵呵好</li>
      <li>98呵呵呵呵好</li>
      <li>99呵呵呵呵好</li>
      <li>100呵呵呵呵好</li>
    </ul>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
export default {
  name: "Category",
  data(){
    return {
      scroll:null
    }
  },
  created() {

  },
  mounted() {
    let bscroll = new BScroll(this.$refs.mywrapper , {
      probeType:3,
      pullUpLoad:true
    })
    bscroll.on('scroll' , (position)=>{
      // console.log(position)
    })
    bscroll.on('pullingUp' , ()=>{
      console.log('上拉加载更多')
      setTimeout(()=>{
        bscroll.finishPullUp()
      },2000)
    })
  },
  methods:{
    clickItem(){
      alert(1)
    }
  }
}
</script>

<style scoped>
  .wrapper{
    height: 150px;
    background-color: red;

    overflow: hidden;
    /*overflow-y: scroll;*/
  }
</style>
