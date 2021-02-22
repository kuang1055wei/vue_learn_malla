<template>
  <div>
    <detail-nav-bar></detail-nav-bar>
    {{id}}
  </div>
</template>

<script>
import DetailNavBar from "@/views/detail/childComps/DetailNavBar";
import {getDetail} from "@/network/detail";

export default {
  name: "Detail",
  data(){
    return {
      id:null
    }
  },
  components:{
    DetailNavBar
  },
  created() {
    this.id = this.$route.params.id
    getDetail(this.id).then(res=>{
      console.log(res)
    })
  },
  activated() {
  }
}
</script>

<style scoped>

</style>
