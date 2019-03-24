<template>
	<view class="main">
		<ul>
			<li class="card-item" v-for="(item,index) in todoList" :key="index">{{item.content}}</li>
		</ul>
	</view>
</template>

<script>
</script>
	export default{
		props:['todoList'],
		data(){
			return{}
		}
			for(let i=0;i<this.todoList.length;i++){
				if(todoList[i].slider>50){
					this.urgent.push(this.todoList[i])
					console.log('pushed')
				}
			}
		}
	}
<style>
	.main{
		
	}
	.card-item{
		color: #FF0000;
		border-bottom: 1px dashed black;
	}
</style>
