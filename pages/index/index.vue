<template>
	<view class="home">
	<scroll-view scroll-x class="navscroll"  >
			<view class="item"  :class="index==navindex?'active':'' " v-for="(item,index) in navarr" 
			:key="item.id" 
			@click="clicknav(index,item.id)" >
			{{item.classname}} 
			</view>
			
		</scroll-view>
		
	
		
		<view class="content">
			<view class="row " v-for="item in newsarr" :key="item.id">
				<newsbox :item="item"  @click.native='godetail(item)'></newsbox>
			</view>
		</view>
		<view class="nodata" v-if="newsarr.length==0">
			<image src="../../static/images/nopic.jpg" mode=""></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navindex:0,
				navarr:[],
				newsarr:[]
			}
		},
		onLoad() {
this.getnavdata();
this.getnewsdata();
		},
		methods: {
           clicknav(index,id){
	            this.navindex=index
				//console.log(id);
				this.getnewsdata(id);
},
			godetail(item){
				uni.navigateTo({
					url:`/pages/detail/detail?cid=${item.classid}&id=${item.id}`
				})
			},
			getnavdata(){
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/navlist.php",
					success:res=>{
						this.navarr=res.data
					}
				})
			},
			getnewsdata(id=50){
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/newslist.php",
					data:{
						cid:id
					},
					success:res=>{
						this.newsarr=res.data
						//console.log(res);
					}
				})
			}

		}
	}
</script>

<style lang="scss" scoped>
	.navscroll{
		height: 100rpx;
		background-color: #F7F8FA;
		white-space: nowrap;
		position: fixed;
		top:var(--window-top);
		left:0;
		z-index: 10;
		/deep/ ::-webkit-scrollbar {
			width: 4px !important;
			height: 1px !important;
			overflow: auto !important;
			background: transparent !important;
			-webkit-appearance: auto !important;
			display: block;
		}
		.item{
			font-size: 40rpx;
			display: inline-block;
			line-height: 100rpx;
			padding:0 30rpx;
			color:#333;		
			&.active{
				color:#31C27C;
			}
		}
	}
	
	.content{
		padding:30rpx;
		padding-top:130rpx;	
		.row{
			border-bottom:1px dotted #efefef;
			padding:20rpx 0;
		}
	}
	.nodata{
		display: flex;
		justify-content: center;
		image{
			width: 360rpx;
		}
	}
	.loading{
		text-align: center;
		font-size: 26rpx;
		color:#888;
		line-height: 2em;
	}
</style>
