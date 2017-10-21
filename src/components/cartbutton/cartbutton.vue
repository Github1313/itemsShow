<template>
	<div>
			<button style="height:20px" @click="add()">+</button>	
				<span>{{food.count}}</span>						
			<button style="height:20px" @click="sub()">-</button>
	</div>

</template>
<script>
import Vue from "vue"
	export default {
		props:['food'],
		methods:{
			add(){
				// console.log(this.food);
				if(!this.food.count){
					Vue.set(this.food,'count',1);
				}else{
					this.food.count++;
				}
				console.log(this.food);
			},
			sub(){
				if(this.food.count){
					this.food.count--;
				}
			}
		}
	}

</script>