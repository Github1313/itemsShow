

<template>
	<transition name="slide">
		<div v-show="showBool" class="food">
			<button @click="showHide"><</button>
		

			<ul>
				<li v-for="item in ratingTitle">
					<button @click="ratingSele(item.reting)">{{item.name}}</button>
				</li>
			</ul>			
			<ul>
				<li v-for="item in ratings">
					{{item.text}}
				</li>
			</ul>


		</div>
	</transition>
</template>

<script>

export default {
	data(){
		return {
			showBool:false,
			ratingTitle:[
				{reting:'All',name:"全部"},
				{reting:0,name:"喜欢"},
				{reting:1,name:"吐槽"},
			],
			ratings:[],
			food:[]

		}
	},
	mounted(){
		// this.ratingSele("All");	
		// setTimeout(()=>{
		// 	console.log(this.food);
		// },5000);
	},
	methods:{

		showHide(food){
			// console.log(food);
			this.food=food.ratings;
			this.showBool=!this.showBool;
			this.ratingSele("All");
		},
		ratingSele(stat){
			// console.log(this.food);
			if(stat=="All"){
				this.ratings=this.food;
				return;
			}
			var rel=[];
			this.food.forEach((item)=>{
				// console.log(item.rateType);
				if(stat==item.rateType){
					// console.log(item);
					rel.push(item);
				}
			})
			this.ratings=rel;
		}
	}

}
</script>

<style scoped lang="scss">
@import "~common/style/mixin.scss";
.food{
	position:fixed;
	top:0;
	left:0;
	bottom:pxrem(100);
	z-index:40;
	width:100%;
	background:#fff;
	transform:translate3d(0,0,0);

}
.slide-enter-active,.slide-leave-active{
	transition:all .3s linear;
}
.slide-leave-active,.slide-enter{
	transform:translate3d(100%,0,0);
}
</style>
