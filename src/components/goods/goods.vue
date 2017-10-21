<template>
  <div class="goods">
  	<!-- 左侧导航 -->
  	<div class="menu-wrapper"  ref="menuWrapper">
  		<ul>
  			<li class="menu-item" 
  				v-for="(item,index) in goods"
				:class="{'current':currentIndex===index}"
  				@click="selectMenu(index)"
  				>{{item.name}}</li>
  		</ul>
  	</div>
  	<!-- 右侧菜单 -->
  	<div class="foods-wrapper" ref="foodsWrapper">
  		<ul>
  			<li class="foods-list" v-for="(item,index) in goods" ref="foodsList">
				<h1 class="title">{{item.name}}</h1>
				<ul>
					<li class="food-item" v-for="food in item.foods">
						<!-- 商品图片 -->
						<div class="icon">
							<img :src="food.icon" @click="_selectFood(food)">
						</div>
						<!-- 商品内容 -->
						<div class="content">
							<h2 class="name">{{food.name}}</h2>
							<p class="desc">{{food.description}}</p>
							<div class="extra">
								<span class="count">月销{{food.sellCount}}</span>
								&nbsp;
								<span>好评率{{food.rating}}%</span>
								<!-- <span>好评率{{}}</span> -->
							</div>
							<div class="price">
								<span class="ico">￥</span><span class="now">{{food.price}}</span>
							</div>
						</div>



						<div class="cartb-wrap" >
							<cartbutton :food='food'></cartbutton>
						</div>
												


					</li>
				</ul>
  			</li>
  		</ul>
  		
  	</div>
  	<cartcontrol :selectFoods="selectFoods" :seller="seller"></cartcontrol>
  	<show  ref="show"></show> 
  </div>
</template>

<script>
import axios from "axios"

import BScroll from 'better-scroll'
import Cartcontrol from "@/components/cartcontrol/cartcontrol"
import cartbutton from "@/components/cartbutton/cartbutton"
import Show from "@/components/show/show"




export default {
	props:{
		seller:{
			type:Object,
		}
	},
	data(){
		return {
			goods:[],
			listHeight:[],
			scrollY:0,
			selectFood:{}

		}
	},
	created(){
		this._getGoods();
	},
	components:{
		Cartcontrol,
		cartbutton,
		Show 
	},
	computed:{
		currentIndex(){
			for(let i=0;i<this.listHeight.length;i++){
				// 分析  第一次 
				let height1=this.listHeight[i];//0
				let height2=this.listHeight[i+1]; //800+
				if(!height2||(this.scrollY>=height1&&this.scrollY<height2)){
					return i;
				}
			}
			return 0;
		},
	 selectFoods(){
	 	let foods=[];
	 	if(this.goods[0]){
	 		this.goods.forEach((val)=>{
	 			// console.log(val);
	 			val.foods.forEach((item)=>{
	 				if(item.count>0){
	 					foods.push(item);
	 				}
	 			})
	 		})
	 	}
	 	console.log(foods);
	 	return foods;
	 }

	},
	methods:{
		_getGoods(){
	        axios.get('api/goods').then((res)=>{
	        	console.log(res.data.data);
	        	this.goods=res.data.data;
	        	// vue dom更新执行回调函数
	        	this.$nextTick(()=>{
	        		this._bscroInit();
	        		this._getFoodsHeight();
	        	})
	     
	        })
		},
		// 1.better-scroll初始化
		_bscroInit(){
			// 取需要滑动的dom节点
			let menuWrapper=this.$refs.menuWrapper;
			let foodsWrapper=this.$refs.foodsWrapper;
			// console.log(menuWrapper);

			// 需要插件加点击事件  需要=设置click:true
			this.meunScroll=new BScroll(menuWrapper,{click:true});
			this.foodsScroll=new BScroll(foodsWrapper,{
				// click:true
				// 属于better-scroll_API 值等于
				//better-scroll 时实滚动触发
				probeType:3
			});
			  //3.scroll动态高度
			this.foodsScroll.on("scroll",(sol)=>{
				// console.log(sol.y);
				// 1.四舍五入转整数 Math.round
				// 2.转成整数Math.abs
				this.scrollY=Math.abs(Math.round(sol.y));
				// console.log(this.scrollY);
				// scroll动态高度  可以使用computed
			})
		},
		// 2.取到商品分类之间高度
	    _getFoodsHeight(){
	    	let foodsList=this.$refs.foodsList;
	    	let height=0;
	    	// console.log(foodsList);
	    	this.listHeight.push(height);

	    	for(let i=0;i<foodsList.length;i++){
	    		// 取到每一个节点
	    		let item=foodsList[i];
	    		// 获取节点高度
	    		height+=item.clientHeight;
	    		this.listHeight.push(height);

	    	}
	    },
	    selectMenu(index){
	    	// console.log(index);
	    	// 属于better-scroll_API,滑动到当前节点,传入参数(节点,动画过度)
	    	let foodsList=this.$refs.foodsList;
	    	let selectEl=foodsList[index];
	    	this.foodsScroll.scrollToElement(selectEl,400);
	    },
	    _selectFood(food){
	    	// console.log(food);
	    	// this.selectFood=food;
			this.$refs.show.showHide(food);

	    }
	  
	  

	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import "~common/style/mixin.scss";
@import "~@/components/goods/goods.scss";
</style>
