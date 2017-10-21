<template>
	<div class="shopcart">
		<div class="content">
			<div class="content-left">
				<div class="logo-wrapper">
					<div class="logo" :style="{background:total.bool?'#25aee1':''}">
						<i class="icon-cart" :class="{on:total.bool}"></i>
					</div>
				</div>
				<div class="num" v-show="total.bool">{{total.num}}</div>
			</div>
			<div class="content-center">
				<span class="price">￥{{total.price}}</span>
				<span class="text">另需配送费￥{{seller.deliveryPrice}}</span>
			</div>
			<div class="content-right">
					{{payDesc}}
			</div>
		</div>
	</div>
</template>
<script>
	export default{
		props:{
			seller:{
				type:Object
			},
			selectFoods:{
				type:Array,
				default(){
					return []
				}
			}
		},
		computed:{
			total(){
				let total={
					price:0,
					num:0,
					bool:false,
					pay:false
				}
				this.selectFoods.forEach((food)=>{
					total.price+=food.count*food.price;
					total.num+=food.count;
				})

				if(total.num>0){
					total.bool=true;
				}
				if(total.price>=20){
					total.pay=true;
				}else{
					total.pay=false;
				}
				return total;
			},
			payDesc(){
				console.log(this.total.price,this.seller.minPrice);
				if (this.total.price===0) {
					return `￥${this.seller.minPrice}元起送`
				}else if (this.total.price<this.seller.minPrice){
					let rel =this.seller.minPrice-this.total.price;
					return `还差￥${rel}元起送`;
				} else {
					return '去结算'
				}

			}
		}
		
	}
</script>
<style scoped lang="scss">
	@import "~common/style/mixin.scss";
	@import "~@/components/cartcontrol/cartcontrol.scss";
</style>