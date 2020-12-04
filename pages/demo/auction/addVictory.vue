<template>
	<view style="display:flex;flex-direction:row;justify-content: space-around;">
		<view v-if="giftshow">
			
				<table>
					<tr>
						<th>装备名称</th>
						<th>装备位置</th>
						<th>属性</th>
						<th>添加</th>
					</tr>
				
					<tr v-for="item in giftType">
						 
						<td>{{item.name}}</td>
						<td>{{item.position}}</td>
						<td>{{item.remark}}</td>
						<td><button class="cu-btn sm bg-blue " @click="addGift(item)">添加</button></td>
						
					</tr>
					
				</table>
			<view class="padding flex flex-direction"><button class="cu-btn bg-red margin-tb-sm lg" @click="completeAdd()">返回</button></view>
			
		</view>
		<view v-for="(item,index) in list" style="text-align: center;"  v-else>
			<view>{{item.name}}</view>
			<button class="cu-btn sm bg-blue" @click="addVictory(item.name)">添加掉落</button>
			<view style="display: flex;flex-direction: column;">
			<view class="padding radius text-center light bg-orange " v-for="(it,index) in item.gift"  style="margin: 10px;" >
				<view>{{it.giftno}}</view>
				<view>当前价格：{{it.price}}</view>
				<view>出价人:{{it.user}}</view>
				<view>最后出价时间：{{it.time}}</view>
				<view v-if="it.status == 1 ? true : false"><button class='cu-btn sm bg-red' @click="deleteGift(it._id)">删除</button></view>
				<view v-else>拍卖已结束</view>
			</view>
			</view>
			<view><button class="cu-btn sm bg-green" style="margin-top: 10upx;" @click="stop(item.name)">结束拍卖</button></view>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return {
				list:[
					{
						name:"余晖",
						gift:[]
					},
					{
						name:"宓桃",
						gift:[]
					},
					{
						name:"武雪散",
						gift:[]
					},
					{
						name:"猿飞",
						gift:[]
					},
					{
						name:"哑头陀",
						gift:[]
					},
					{
						name:"岳琳",
						gift:[]
					}
				],
				giftType:[],
				giftshow:false,
				addgiftlist:[],
				boss:''
			}
		},
		methods:{
			getCarInfo(){
				this.giftshow=false
				this.list[0].gift=[]
				this.list[1].gift=[]
				this.list[2].gift=[]
				this.list[3].gift=[]
				this.list[4].gift=[]
				this.list[5].gift=[]
				const db = uniCloud.database()
				db.collection('gift').where({
					number:this.number
				}).get().then((res) => {
					
					const data=res.result.data;
				
					for(var i=0;i<data.length;i++){
						if(data[i].boss == '余晖'){
							this.list[0].gift.push(data[i])
						}else if(data[i].boss == '宓桃'){
							this.list[1].gift.push(data[i])
						}
						else if(data[i].boss == '武雪散'){
							this.list[2].gift.push(data[i])
						}
						else if(data[i].boss == '猿飞'){
							this.list[3].gift.push(data[i])
						}
						else if(data[i].boss == '哑头陀'){
							this.list[4].gift.push(data[i])
						}
						else if(data[i].boss == '岳琳'){
							this.list[5].gift.push(data[i])
						}
					}
				})
			},
			addVictory(e){
				this.boss=e
				const db = uniCloud.database()
				db.collection('giftType').where({
					boss:new RegExp(e+'?')
				}).get().then((res) => {				
					this.giftType=res.result.data
					this.giftshow=true
				})
			},
			addGift(e){
				const db = uniCloud.database()
				uni.showLoading({
					
				})
				db.collection('gift').add([{
					number:this.number,
					boss:this.boss,
					giftno:e.name,
					price:e.price,
					status:'1'
				}]).then((res)=>{
					uni.hideLoading()
					uni.showToast({
						title:'添加'+e.name+'成功',
						icon:'success',
						duration:1000
					})
				})
				
			
				
			},
			completeAdd(){
				var _this=this
				// uni.showLoading({
					
				// })
				// setTimeout(function(){
				// 	uni.hideLoading()
					
				// },2000)
				_this.getCarInfo()
				
			},
			deleteGift(e){
				const db = uniCloud.database()
				db.collection("gift").doc(e).remove().then((res) =>{
					uni.showToast({
						title:'删除成功'
					})
					this.getCarInfo()
				})
				
			},
			stop(e){
				const db = uniCloud.database()
				uni.showLoading({
					
				})
				if(e == '余晖'){
					
					for(var i=0;i<this.list[0].gift.length;i++){
					
						db.collection("gift").where({_id:this.list[0].gift[i]._id}).update({
							status:'0'
						})
					}
					uni.hideLoading()
					this.getCarInfo()
				}else if(e == '宓桃'){
					
					for(var i=0;i<this.list[1].gift.length;i++){
						db.collection("gift").where({_id:this.list[1].gift[i]._id}).update({
							status:'0'
						})
					}
					uni.hideLoading()
					this.getCarInfo()
				}else if(e == '武雪散'){
					
					for(var i=0;i<this.list[2].gift.length;i++){
						db.collection("gift").where({_id:this.list[2].gift[i]._id}).update({
							status:'0'
						})
					}
					uni.hideLoading()
					this.getCarInfo()
				}else if(e == '猿飞'){
					for(var i=0;i<this.list[3].gift.length;i++){
						db.collection("gift").where({_id:this.list[3].gift[i]._id}).update({
							status:'0'
						})
					}
					this.getCarInfo()
				}else if(e == "哑头陀"){
					
					for(var i=0;i<this.list[4].gift.length;i++){
						db.collection("gift").where({_id:this.list[4].gift[i]._id}).update({
							status:'0'
						})
					}
					uni.hideLoading()
					this.getCarInfo()
				}else if(e == "岳琳"){
					for(var i=0;i<this.list[5].gift.length;i++){
						db.collection("gift").where({_id:this.list[5].gift[i]._id}).update({
							status:'0'
						})
					}
					uni.hideLoading()
					this.getCarInfo()
					
				}
			}
		},
		onLoad(e){
			this.number = e.no,
			this.getCarInfo()
		}
	}
</script>

<style>
</style>
