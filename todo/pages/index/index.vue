<template>
	<view class="content">
		<form @submit="formsubmit" @reset="formReset">
			<view class="input-space">
				<input class="uni-input" name="input" placeholder="输入TODO" focus confirm-type="next" v-model="ctx"/>
				<view @click="show">Less</view>
			</view>
			<button @click="save">submit</button>
			<view class="setting" v-show="showDetail">
				<view class="level-hint">紧急程度</view>
				<view class="level">	
				 <radio-group class="labels" @change="radioChange">
					 <label class="radio"><radio value="1" />Level 1</label>
					 <label class="radio"><radio value="2" />Level 2</label>
					 <label class="radio"><radio value="3" />Level 3</label>
					 <label class="radio"><radio value="4" checked="true" />Level 4</label>
				 </radio-group>
				</view>
				 <view class="easy-hard">
					 <view>重要程度</view>
					 <slider value="50" @change="sliderChange" show-value />
				 </view>
				<view class="date-picker">
					<view class="date-hint">完成日期</view>
					<view>
					 	<picker mode="date" :value="date" :start="startDate" :end="endDate" @change="dateChange">
					 		<view class="uni-input">{{date}}</view>
					 	</picker>
					</view>
				</view>
			</view>
		</form>
		<view class="list-title">最近需要处理的事情：</view>
		<view>
			<ul>
				<li v-for="(item,index) in showList" :key="index">
					<view class="li-content">
						<view class="content-hidden">{{item.content}}</view>
						<view class="slider-hidden">{{item.slider}}</view>
						<view class="level-hidden">{{item.level}}</view>
					</view>
				</li>
			</ul>
		</view>
	</view>
</template>

<script>
	const user = 'localUser'
	export default {
		data(){
			const currentDate = this.getDate({
				format:true
			});
			return {
				date:currentDate,
				showDetail: true,
				todoList:[],
				ctx:'',
				state:{
					content:'',
					level:'4',
					date: currentDate,
					slider:50,
					done: false
				}
			}
		},
		computed:{
			startDate(){
				return this.getDate('start')
			},
			endDate(){
				return this.getDate('end')
			},
			showList(){
				if(this.todoList.length<3){
					return this.todoList
				} else {
					return this.todoList.slice(-3)
				}
			}
		},
		created(){
			// this.getSync()
			this.get()
		},
		methods:{
			get(){
				// get之前先set不是常识吗
				uni.setStorage({
					key: user,
					data: this.todoList
				})
				let self = this
				uni.getStorage({
				  key: user,
				  success(res) {
					console.log('success')
					console.log(res.data)
					self.todoList = res.data
					console.log(self.todoList)
				  },
				  fail(){
					  console.log('fail')
				  },
				  complete(){
					  console.log('end')
				  }
				})
			},
			getSync(){
				const res = uni.getStorageSync(user)
				if(res){
					this.list = res.data
					console.log('success in get')
				} else {
					console.log('error in get')
				}
			},
			save(){
				// 维护时间重置器
				const currentDate1 = this.getDate({
					format:true
				});
				if(this.ctx){
					this.state.content = this.ctx
					this.todoList.push(this.state)
					console.log(this.todoList)
					uni.setStorage({
						key: user,
						data: this.todoList
					})
					console.log('saved')
					this.state = {
						content:'',
						level:'4',
						date: currentDate1,
						slider:50,
						done: false
					},
					this.ctx=''
					console.log('clear the label')
					// 在保存之后重新获取
					this.get()
				} else {
					console.log('空内容')
				}
			},
			show(){
				console.log("click")
				this.showDetail = !this.showDetail;
			},
			sliderChange(e){
				console.log(e.detail.value)
				this.state.slider = e.detail.value
				return e.detail.value
			},
			dateChange(e){
				this.state.date = e.target.value
				console.log(e.target.value)
			},
			radioChange(e){
				this.state.level = e.detail.value
// 				if(e.detail.value == 'r1'){
// 					this.state.level = '1'
// 				}
// 				if(e.detail.value == 'r2'){
// 					this.state.level = '2'
// 				}
// 				if(e.detail.value == 'r3'){
// 					this.state.level = '3'
// 				}
// 				if(e.detail.value == 'r4'){
// 					this.state.level = '4'
// 				}
				console.log(e.detail.value)
			},
			getDate(type) {
				const date = new Date();
			
				let year = date.getFullYear();
				let month = date.getMonth() + 1;
				let day = date.getDate();
			
				if (type === 'start') {
					year = year - 60;
				} else if (type === 'end') {
					year = year + 2;
				}
				month = month > 9 ? month : '0' + month;;
				day = day > 9 ? day : '0' + day;
				
				return `${year}-${month}-${day}`;
			}
		}
	}
</script>

<style>
	.content{
		background-color: #f3f5f7;
		padding: 20px;
		font-size: $uni-font-size-base;
		color: black;
	}
	.input-space{
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		border-bottom:2px dashed rgb(67,144,87);
		margin-bottom: 10px;
	}
	.setting{
		margin: 20px 0 ;
	}
	.level{
		font-size: 16px;
		margin-bottom: 10px;
	}
	.level-hint{
		font-size: $uni-font-size-base;
	}
	.labels{
		display: flex;
		flex-direction: row;
		justify-content: sapce-around;
		align-items: center;
	}
	.easy-hard{}
	.date-picker{
		display: flex;
		flex-direction: row;
		justify-content: flex-start;
	}
	.date-hint{
		flex: 0 0 130px;
	}
	.list-title{
		margin-top: 10px;
		margin-bottom: 10px;
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
	}
	.slider-hidden{
		flex: 0 0 40px
	}
	.level-hidden{
		flex: 0 0 40px
	}
	.done{
		text-decoration: line-through;
		color: rgb(121,121,121);
	}
</style>
