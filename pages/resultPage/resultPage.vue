<template>
	<view class="resultPage-warp">
		<view class="completeImg">
			<img src="../../static/result-img.svg">
		</view>
		<view class="tit-complete">
			已全部完成
		</view>
		<view class="complete-info">
			<span class="userName">姓名：{{userName}}</span>
			<span class="userid">学号：{{userId}}</span>
			<span class="comTime">完成时间：{{comTime}}</span>
		</view>
		<view class="resultA commonTableStyle">
			<view class="resultA-header commonFlexStyle">
				<span class="resultA-header-item commonTableBorder">CP</span>
				<span class="resultA-header-item commonTableBorder">NP</span>
				<span class="resultA-header-item commonTableBorder">A</span>
				<span class="resultA-header-item commonTableBorder">FC</span>
				<span class="resultA-header-item commonTableBorder">AC</span>
			</view>
			<view class="resultA-content commonFlexStyle">
				<span class="resultA-td-item commonTableBorder">{{ CP }}</span>
				<span class="resultA-td-item commonTableBorder">{{ NP }}</span>
				<span class="resultA-td-item commonTableBorder">{{ _A }}</span>
				<span class="resultA-td-item commonTableBorder">{{ FC }}</span>
				<span class="resultA-td-item commonTableBorder">{{ AC }}</span>
			</view>
		</view>

		<view class="resultB commonTableStyle">
			<view class="resultB-header commonFlexStyle">
				<span class="resultB-header-item commonTableBorder">P</span>
				<span class="resultB-header-item commonTableBorder">A</span>
				<span class="resultB-header-item commonTableBorder">C</span>
			</view>
			<view class="resultB-content commonFlexStyle">
				<span class="resultB-td-item commonTableBorder">{{ P }}</span>
				<span class="resultB-td-item commonTableBorder">{{ A }}</span>
				<span class="resultB-td-item commonTableBorder">{{ C }}</span>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				P: 0,
				A: 0,
				C: 0,
				CP: 0,
				NP: 0,
				FC: 0,
				AC: 0,
				_A: 0,
				userName: '',
				userId: '',
				comTime: ''
			};
		},
		methods: {
			checkTime(i) {
				if (i < 10) {
					i = "0" + i;
				}
				return i;
			}
		},
		created() {
			const nowTime = new Date()
			var year = nowTime.getFullYear(),
				month = nowTime.getMonth() + 1,
				day = nowTime.getDay(),
				h = this.checkTime(nowTime.getHours()),
				m = this.checkTime(nowTime.getMinutes()),
				s = this.checkTime(nowTime.getSeconds())
			this.comTime = year + "年" + month + "月" + day + "日 " + h + ":" + m + ":" + s;
		},
		onLoad(options) {
			// console.log(options.resultScore);
			const res = JSON.parse(options.resultScore)
			this.P = res.P
			this.A = res.A
			this.C = res.C
			this.CP = res.CP
			this.NP = res.NP
			this.FC = res.FC
			this.AC = res.AC
			this._A = res._A

			this.userName = options.username
			this.userId = options.userid
		}
	}
</script>

<style lang="scss">
	.resultPage-warp {
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
	}

	.completeImg {
		height: 320rpx;
	}

	.complete-info {
		margin-top: 30rpx;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}

	.tit-complete {
		color: #f08c1f;
		font-weight: 800;
		font-size: 35rpx;
		margin-top: 20rpx;
	}

	.commonTableStyle {
		display: flex;
		flex-direction: column;
		justify-items: center;
		align-items: center;
		padding-top: 70rpx;
	}

	.commonTableBorder {
		flex: 1;
		height: 70rpx;
		line-height: 70rpx;
		text-align: center;
		border: 2px solid #ebeef5;
	}

	.commonFlexStyle {
		width: 80vw;
		display: flex;
		justify-content: space-between;
	}

	.resultA {
		.resultA-header {
			.resultA-header-item {
				font-weight: 800;
				color: #f08c1f;
				border-bottom: none;
			}
		}

		.resultA-content {
			.resultA-td-item {
				border: 2px solid #ebeef5;
			}
		}
	}

	.resultB {
		.resultB-header {
			.resultB-header-item {
				font-weight: 800;
				color: #f08c1f;
				border-bottom: none;
			}
		}
	}

</style>
