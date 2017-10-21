<template>
  
  <div class="tab">
  		<div class="tab-item" v-for="(item,index) in tabInfo">
  			<router-link :to="item.path" :key="index" v-text="item.name"></router-link>
  		</div>
  </div>
</template>

<script>
export default {
	data(){
		return{
			tabInfo:[
				{"name":"商品","path":"/goods"},
				{"name":"评价","path":"/ratings"},
				{"name":"商家","path":"/seller"}
			]
		}
		
	}
}
</script>
<style scoped lang="scss">
@import "~common/style/mixin.scss";
.tab{
	height:pxrem(80);
	display:flex;
	text-align:center;
	line-height:pxrem(80);
	border-bottom:1px solid rgba(7,17,27,0.1);
	.tab-item{
		flex:1;
		a{
			@include font-dpr(16px);
			padding:10px;
		}
		& .active{
			color:rgb(240,20,20);
		}
	}
}

</style>
