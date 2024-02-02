<template>
	<view class="content">
		<view class="testIconContent">
			<view class="iconWarp">
				<img class="logo" src="../../static/icon_index.svg">
			</view>
		</view>
		<view class="userInfoWarp">
			<view class="userName commonWarp">
				<span class="tit-input">姓名:</span>
				<input type="text" :value="username" id="username"
					:class="isWarnUsername?'warn-input common-input':'common-input'" @blur="InputBlurHandler(1)"
					@input="(e)=>inputCode(e, 1)">
			</view>
			<view class="warnUsername common-warn" v-if="isWarnUsername">{{ nameMsg }}</view>
			<view class="userid commonWarp">
				<span class="tit-input">学号: </span>
				<input type="text" id="userid" :value="userid"
					:class="isWarnUserid?'warn-input common-input':'common-input'" @blur="InputBlurHandler(0)"
					@input="(e)=>inputCode(e, 0)">
			</view>
			<view class="warnUserid common-warn" v-if="isWarnUserid">{{ idMsg }}</view>
		</view>
		<view class="btnWarp">
			<button @click="GoToTest()" class="startBtn">立即开始</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				username: '',
				userid: '',
				isWarnUsername: false,
				isWarnUserid: false,
				idMsg: '学号不可以为空!',
				nameMsg: '姓名不可以为空!'
			}
		},
		onLoad() {

		},
		methods: {
			inputCode(e, type) {
				const inputVal = e.detail.value
				if (type === 1) {
					this.username = inputVal
				} else if (type === 0) {
					this.userid = inputVal
				}
			},
			CheckRge(type) {
				const _this = this
				if (type === 1) {
					if (/[\u4e00-\u9fa5]{2,4}/.test(_this.username)) {
						return true
					} else {
						return false
					}
				}

				if (type === 0) {
					if (/[0-9]{10}/.test(_this.userid)) {
						return true
					} else {
						return false
					}
				}

			},
			InputBlurHandler(type) {
				const _this = this
				//name
				if (type === 1) {
					if (_this.username === '') {
						_this.nameMsg = "姓名不可以为空!"
						_this.isWarnUsername = true
						return false
					} else {
						if (_this.CheckRge(1)) {
							_this.isWarnUsername = false
							return true
						} else {
							_this.nameMsg = "姓名格式不正确"
							_this.isWarnUsername = true
							return false
						}
					}
					return
				}
				//id
				if (type === 0) {
					if (_this.userid === '') {
						_this.idMsg = "学号不可以为空!"
						_this.isWarnUserid = true
						return false
					} else {
						if (_this.CheckRge(0)) {
							_this.isWarnUserid = false
							return true
						} else {
							_this.idMsg = "学号格式不正确"
							_this.isWarnUserid = true
							return false
						}
					}
				}

			},
			GoToTest() {
				const _this = this
				const nameCheckedRes = _this.InputBlurHandler(1)
				const isCheckedRes = _this.InputBlurHandler(0)
				if (nameCheckedRes && isCheckedRes) {
					uni.navigateTo({
						url: "/pages/test/test?userName=" + this.username + "&userId=" + this.userid
					})
				}
			}
		}
	}
</script>

<style lang="scss">
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.testIconContent {
		margin-top: 200rpx;

		.logo {
			height: 200rpx;
			width: 200rpx;
		}

		.iconWarp {
			display: flex;
			align-items: center;
			justify-content: center;
			height: 300rpx;
			width: 300rpx;
			border-radius: 100rpx;
			background-color: #fff;
			box-shadow: 0px 0px 20px rgba(153, 153, 153, 0.3);
		}
	}

	.userInfoWarp {
		display: flex;
		flex-direction: column;
		margin-top: 40rpx;
		font-size: 35rpx;

		.tit-input {
			width: 100rpx;
			color: #666;
			margin-right: 20rpx;
		}

		.common-warn {
			font-size: 28rpx;
			color: #fe6c6f;
			padding-left: 120rpx;
		}

		.commonWarp {
			margin-top: 20rpx;
			margin-bottom: 20rpx;
			display: flex;
			align-items: center;

			.common-input {
				width: 270rpx;
				outline: none;
				padding-left: 20rpx;
				border: 1px solid #dcdfe6;
				border-radius: 5px;
				height: 70rpx;
			}

			.warn-input {
				border: 2px solid #fe6c6f;
			}
		}
	}

	.btnWarp {
		margin-top: 50rpx;

		.startBtn {
			background-color: #f08c1f;
			color: #fff;
			box-shadow: 0px 0px 10px 0px rgba(240, 140, 31, 0.5);
		}
	}
</style>
