<template>
	<view class="page-list">
		<view class="tab">
			<view @click="sortedBySlider">重要程度</view>
			<view id="line">|</view>
			<view @click="sortedByLevel">紧急程度</view>
		</view>
		<view class="list-content">
			<ul>
				<li class="li-title">
					<span id="content1">内容</span>
					<span id="content2">日期</span>
				</li>
				<li v-for="(item,index) in oList" :key="index" @click="done(item)" :class="{done:item.done}">
					<view class="li-content">
						<view class="content-hidden">{{item.content}}</view>
						<view class="date-hidden">{{item.date}}</view>
					</view>
				</li>
			</ul>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				uList:[],
				oList:[],
				doneList:[]
			}
		},
		onLoad(){
			this.get()
		},
		methods:{
			done(item){
				item.done = true
				this.doneList.push(item)
				uni.setStorage({
					key: 'done',
					data: this.doneList
				})
			},
			sortedBySlider(){
				this.oList = this.uList.sort(function(a,b){
					return b.slider-a.slider
				})
			},
			sortedByLevel(){
				this.oList = this.uList.sort(function(a,b){
					return b.level - a.level
				})
			},
			get(){
				let self = this
				uni.getStorage({
					key:'localUser',
					success(res) {
						console.log('success')
						self.uList = res.data
						self.oList = res.data
						console.log(self.oList)
					},
					fail(){
					  console.log('fail')
					},
					complete(){
					  console.log('end')
					}
				})
			}
		}
	}
</script>

<style>
	.page-list{
		pading:20px;
	}
	.tab{
		position: fixed;
		width: 100%;
		height: 60px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: space-around;
		border-bottom: 1px dashed rgb(67,144,87);
		background-color: #f8f8f8;
	}
	.list-content{
		position: absolute;
		left: 0px;
		top: 60px;
		width: 100%;
		overflow: scroll;
	}
	#line{
		color: rgba(67,144,87,0.5);
	}
	.li-title{
		font-size: $uni-font-size-sm;
		color: rgb(121,121,121);
		display: flex;
		justify-content: space-around;
		margin: 10px 0;
	}
	#content1{
		flex: 1;
		padding-left: 50px;
	}
	#content2{
		flex: 0 0 150px;
		text-align:center;
	}
	.li-content{
		display: flex;
		flex-direction: row;
		justify-content: space-around;
		border-bottom: 1px solid rgb(67,144,87);
		margin-bottom: 5px;
	}
	.content-hidden{
		flex: 1;
		text-overflow: ellipsis;
		white-space: nowrap;
		overflow: hidden;
		padding-left: 50px;
	}
	.date-hidden{
		flex: 0 0 150px;
		text-align:center;
	}
	.done{
		text-decoration: line-through;
		color: rgb(121,121,121);
	}
</style>
