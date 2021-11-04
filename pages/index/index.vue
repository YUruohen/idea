<template>
	<view>
		<view class="contain">
			<view class="head">
				<view class="title">{{title}}</view>
				<view class="fast">
					<view class="kuai"></view>
				</view>
			</view>
		</view>

		<view class="search">
			<u-search shape="round" :clearabled="true" height="90"></u-search>
		</view>

		<view class="swiper">
			<u-swiper :list="imgList"></u-swiper>
		</view>

		<view style="height: 150rpx;">
			<u-grid :col="4" :border="false">
				<u-grid-item>
					<u-badge count="9" :offset="[20, 20]"></u-badge>
					<u-icon name="photo" :size="46"></u-icon>
					<view class="grid-text">限时折扣</view>
				</u-grid-item>
				<u-grid-item>
					<image src="/static/image/icon/hot5.png" class="badge-icon"></image>
					<u-icon name="lock" :size="46"></u-icon>
					<view class="grid-text">品牌大全</view>
				</u-grid-item>
				<u-grid-item>
					<u-icon name="hourglass" :size="46"></u-icon>
					<view class="grid-text">日常用品</view>
				</u-grid-item>
				<u-grid-item>
					<u-icon name="hourglass" :size="46"></u-icon>
					<view class="grid-text">更多</view>
				</u-grid-item>
			</u-grid>
		</view>

		<view style="position: relative;" v-if="show">
			<view style="position: absolute;right: 30rpx;z-index:99;color:white;top:20rpx;" class="X" @click="showAd()">X</view>
			<u-image width="100%" height="300rpx" :src="img"></u-image>
		</view>
		<view v-else @click="showAd()">恢复广告</view>


		<view>
			<u-notice-bar type="primary" :list="noList" :more-icon="false" :autoplay="false"></u-notice-bar>
		</view>

		<view>

			<view class="wrap">
				<u-button @click="clear">清空列表</u-button>
				<u-waterfall v-model="flowList" ref="uWaterfall">
					<template v-slot:left="{leftList}">
						<view class="demo-warter" v-for="(item, index) in leftList" :key="index">
							<!-- 警告：微信小程序中需要hx2.8.11版本才支持在template中结合其他组件，比如下方的lazy-load组件 -->
							<u-lazy-load threshold="-450" border-radius="10" :image="item.image" :index="index">
							</u-lazy-load>
							<view class="demo-title">
								{{item.title}}
							</view>
							<view class="demo-price">
								{{item.price}}元
							</view>
							<view class="demo-tag">
								<view class="demo-tag-owner">
									自营
								</view>
								<view class="demo-tag-text">
									放心购
								</view>
							</view>
							<view class="demo-shop">
								{{item.shop}}
							</view>
							<u-icon name="close-circle-fill" color="#fa3534" size="34" class="u-close"
								@click="remove(item.id)"></u-icon>
						</view>
					</template>
					<template v-slot:right="{rightList}">
						<view class="demo-warter" v-for="(item, index) in rightList" :key="index">
							<u-lazy-load threshold="-450" border-radius="10" :image="item.image" :index="index">
							</u-lazy-load>
							<view class="demo-title">
								{{item.title}}
							</view>
							<view class="demo-price">
								{{item.price}}元
							</view>
							<view class="demo-tag">
								<view class="demo-tag-owner">
									自营
								</view>
								<view class="demo-tag-text">
									放心购
								</view>
							</view>
							<view class="demo-shop">
								{{item.shop}}
							</view>
							<u-icon name="close-circle-fill" color="#fa3534" size="34" class="u-close"
								@click="remove(item.id)"></u-icon>
						</view>
					</template>
				</u-waterfall>
				<u-loadmore bg-color="rgb(240, 240, 240)" :status="loadStatus" @loadmore="addRandomData"></u-loadmore>
			</view>

		</view>
		
		<view class="footer">
			<view class="top">
				<view class="login">登录</view>
				<view class="buy">购物车</view>
				<view class="kefu">客服</view>
			</view>
			<view class="botton">
				<view class="copy">版权所有@dark2021粤0001号</view>
			</view>
		</view>


	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: "爽歪歪",
				imgList: [{
						image: '/static/img/bg.jpg',
						title: '图1',
					},
					{
						image: '/static/img/bg.jpg',
						title: '图2',
					},
				],
				img: "/static/img/bg.jpg",
				noList: [
					"限时折扣"
				],
				flowList: [],
				loadStatus: 'loadmore',
				list: [{
						price: 35,
						title: '北国风光，千里冰封，万里雪飘',
						shop: '李白杜甫白居易旗舰店',
						image: 'http://pic.sc.chinaz.com/Files/pic/pic9/202002/zzpic23327_s.jpg',
					},
					{
						price: 75,
						title: '望长城内外，惟余莽莽',
						shop: '李白杜甫白居易旗舰店',
						image: 'http://pic.sc.chinaz.com/Files/pic/pic9/202002/zzpic23325_s.jpg',
					},
				],
				show:true,
			}
		},
		onLoad() {
			this.addRandomData();
		},
		// 触底
		onReachBottom() {
			this.loadStatus = 'loading';
			// 模拟数据加载
			setTimeout(() => {
				// this.addRandomData();
				this.loadStatus = 'loadmore';
			}, 1000)
		},
		methods: {
			// 显示关闭广告
			showAd(){
				console.log("is");
				this.show = !this.show
				console.log(this.show);
			},
			addRandomData() {
				for (let i = 0; i < 4; i++) {
					let index = this.$u.random(0, this.list.length - 1);
					// 先转成字符串再转成对象，避免数组对象引用导致数据混乱
					let item = JSON.parse(JSON.stringify(this.list[index]))
					item.id = this.$u.guid();
					this.flowList.push(item);
				}
			},
			remove(id) {
				this.$refs.uWaterfall.remove(id);
			},
			clear() {
				this.$refs.uWaterfall.clear();
			}
		}
	}
</script>

<style>
	.head {
		display: flex;
		height: 120rpx;
		background-color: #ccc5c3;
		align-items: center;
		justify-content: space-between;
	}

	.title {
		flex: 1;
		color: white;
		font-size: 36rpx;
		margin-left: 20rpx;
	}

	.fast {
		flex: 1;
		display: flex;
		justify-content: flex-end;
	}

	.kuai {
		width: 60rpx;
		height: 56rpx;
		border-top: 32rpx double;
		border-bottom: 12rpx solid;
		margin-right: 40rpx;
	}

	.search {
		margin-top: 20rpx;
	}

	.swiper {
		margin-top: 20rpx;
	}

	.badge-icon {
		position: absolute;
		top: 14rpx;
		right: 40rpx;
		width: 30rpx;
		height: 30rpx;
	}

	.grid-text {
		font-size: 28rpx;
		margin-top: 4rpx;
		color: $u-type-info;
	}
	.footer {
		display:flex;
		height:150rpx;
		background-color: #ccc5c3;
		flex-direction: column;
	}
	.footer .top {
		height:75rpx;
		display: flex;
		color: white;
	}
	.top view {
		flex:1;
		text-align: center;
		line-height: 75rpx;
	}
	.top .login,.buy {
		border-right: 1px solid white;
		border-bottom: 1px solid white;
	}
	.top .kefu {
		border-bottom: 1px solid white;
	}
	
	.footer .botton {
		display: flex;
		height:75rpx;
		color:white;
	}
	.copy {
		flex: 1;
		text-align:center;
		line-height: 75rpx;
	}
</style>

<style lang="scss" scoped>
	.demo-warter {
		border-radius: 8px;
		margin: 5px;
		background-color: #ffffff;
		padding: 8px;
		position: relative;
	}

	.u-close {
		position: absolute;
		top: 32rpx;
		right: 32rpx;
	}

	.demo-image {
		width: 100%;
		border-radius: 4px;
	}

	.demo-title {
		font-size: 30rpx;
		margin-top: 5px;
		color: $u-main-color;
	}

	.demo-tag {
		display: flex;
		margin-top: 5px;
	}

	.demo-tag-owner {
		background-color: $u-type-error;
		color: #FFFFFF;
		display: flex;
		align-items: center;
		padding: 4rpx 14rpx;
		border-radius: 50rpx;
		font-size: 20rpx;
		line-height: 1;
	}

	.demo-tag-text {
		border: 1px solid $u-type-primary;
		color: $u-type-primary;
		margin-left: 10px;
		border-radius: 50rpx;
		line-height: 1;
		padding: 4rpx 14rpx;
		display: flex;
		align-items: center;
		border-radius: 50rpx;
		font-size: 20rpx;
	}

	.demo-price {
		font-size: 30rpx;
		color: $u-type-error;
		margin-top: 5px;
	}

	.demo-shop {
		font-size: 22rpx;
		color: $u-tips-color;
		margin-top: 5px;
	}
</style>
