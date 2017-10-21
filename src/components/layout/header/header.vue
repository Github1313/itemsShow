<template>
  <div class="header">
    <!-- 左侧图片 -->
    <div class="goods_pic">
      <div class="pic">
        <img :src="seller.avatar" alt="">
      </div>
    </div>
    <!-- 右侧描述 -->
    <div class="content">
        <div class="title">
            <span class="name">{{seller.name}}</span>
        </div>
        <div class="descr">
            <span class="text">{{seller.description}}/{{seller.deliveryTime}}分钟送达</span>
        </div>
         <div class="supports" v-if="seller.supports">
            <span class="text">{{seller.supports[0].description}}</span>
        </div>

      

    </div>
    <div class="button-bar">
        <div class="pic"></div>
        <div class="text">{{seller.bulletin}}</div>
        <div class="icon">
          <i class="icon-right"></i>
        </div>
    </div>

    <div class="bg">
      <img :src="seller.avatar" width="100%" height="100%">
    </div>
  </div>
</template>

<script>
export default {
  // props验证
    props:{
      seller:{
        type:Object,
      }
    }
}
</script>
<style scoped lang="scss">
@import "~common/style/mixin.scss";
@import "~@/components/layout/header/header.scss";
</style>
