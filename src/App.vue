<template>
  <div id="app" v-if="seller.data">
    <m-header :seller="seller.data"></m-header>
    <tab></tab>
    <router-view  :seller="seller.data"></router-view>
  </div>
</template>

<script>
import axios from 'axios'
import Tab from "@/components/layout/tab/tab"
import MHeader from "@/components/layout/header/header"

export default {
  name: 'app',
  data(){
    return {
      seller:{},
      name:"Msea"
    }
  },
  components:{
    MHeader,
    Tab
  },
  created(){
    this.__getSeller()
  },
  methods:{
    __getSeller(){
        axios.get('api/seller').then((res)=>{
            this.seller=res.data;
        })
      }
  }
}
</script>

<style>

</style>
