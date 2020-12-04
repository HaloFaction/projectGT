<template>
	<view class="uni-group" style="display: flex;flex-direction: column;">
		
		<view class="cu-form-group">
			<view class="title">装备名字</view>
			<input placeholder="" v-model="name"></input>
		</view>
		<view class="cu-form-group">
			<view class="title">装备部位</view>
			<input type="text" v-model="position"/>
		</view>
		<view class="cu-form-group">
			<view class="title">装备描述</view>
			<input type="text" v-model="remark"/>
		</view>
		<view class="cu-form-group">
			<view class="title">掉落boss</view>
			<input type="text" v-model="boss"/>
		</view>
		<view class="cu-form-group">
			<view class="title">最低价格</view>
			<input type="number" v-model="price"/>
		</view>
		<button class="cu-btn lg bg-blue" @click="submit()">提交</button>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				name:'',
				position:'',
				remark:'',
				boss:'',
				price:''
			}
		},
		methods:{
			submit(){
				const db = uniCloud.database()
				db.collection('giftType').add([{
						name:this.name,
						position:this.position,
						remark:this.remark,
						boss:this.boss,
						price:this.price
					}]).then((res) => {
						uni.showToast({
							title: '新增成功'
						})
					})
					.catch((err) => {
						uni.showModal({
							content: err.message || '新增失败',
							showCancel: false
						})
					})
					.finally(() => {
						
					})
				
			}
		}
	}
</script>

<style>
</style>
