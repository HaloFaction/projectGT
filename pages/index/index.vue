<template>
    <scroll-view style="padding: 15px;box-sizing: border-box;">
		<!-- #ifndef H5 -->
		<fix-window />
		<!-- #endif -->
		<view v-if="show">
			<view class="data-list" >
				<view class="data-info">Boss</view><view class="data-info">状态</view>
			</view>
			<view v-for="(item,index) in list">
				<view class='data-list'>
					<view class="data-info">
						{{item.name}}
					</view>
					<view class="data-info" @click="start(item.status)" style="cursor: pointer;">
						{{item.status == 0?"未击杀":"拍卖中"}}
					</view>
				</view>
			</view>
			
		</view>
    </scroll-view>
</template>

<script>
	
    export default {
        data() {
            return {
				show:false,
				list:[],
				index: 0,
			}
        },
        onLoad() {
			const db=uniCloud.database()
			db.collection('boss').get().then((res) =>{
				this.list = res.result.data
			})
		},
        methods: {
			start(e){
				console.log(e)
				if(e == 0){
					uni.showToast({
						title:'请等待boss击杀完成',
						icon:'none',
						duration:2000
					})
				}else{
					uni.navigateTo({
						url:'../demo/auction/auction'
					})
				}
			},
			
		}
    }
</script>

<style>
	/* #ifndef H5 */
	page {
		padding-top: 85px;
	}
	/* #endif */
	.data-list{
		width: 100%;
		display: flex;
		justify-content: space-around;
		margin-bottom: 20px;
	}
	.data-info{
		width: 50%;
		text-align: center;
	}
</style>
