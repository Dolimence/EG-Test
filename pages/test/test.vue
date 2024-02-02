<template>
	<view class="testWarp">
		<view class="testIngWarp" v-if="!isCacuate">
			<view class="indexWarp">
				<span class="currentIndex">
					{{ currentQuestion + 1 }}
				</span>
				<span class="totalNum"> / {{ totalQuestion }} </span>
			</view>
			<view class="questionWarp">
				{{ questions[currentQuestion] }}
			</view>
			<view class="textWarp">
				<view class="testTitA">
					请选择分数:
				</view>
				<view class="testTit">
					完全不符合1分 不符合2分 一般3分 符合4分 完全符合5分
				</view>
			</view>
			<view class="scoreWarp">
				<span :class="scoreList[currentQuestion]==1?'scoreItem':'noSekected'"
					@click="AddScore(currentQuestion, 1)">1</span>
				<span :class="scoreList[currentQuestion]==2?'scoreItem':'noSekected'"
					@click="AddScore(currentQuestion, 2)">2</span>
				<span :class="scoreList[currentQuestion]==3?'scoreItem':'noSekected'"
					@click="AddScore(currentQuestion, 3)">3</span>
				<span :class="scoreList[currentQuestion]==4?'scoreItem':'noSekected'"
					@click="AddScore(currentQuestion, 4)">4</span>
				<span :class="scoreList[currentQuestion]==5?'scoreItem':'noSekected'"
					@click="AddScore(currentQuestion, 5)">5</span>
			</view>
			<view class="boardWarp">
				<span class="boardTit">答题卡</span>
				<scroll-view scroll-y="true" class="boardScorll">
					<view @click="GoToQuestionByIndex(i)" :class="item < 0?'boardItem':'finishedItem'"
						v-for="(item, i) in scoreList">{{ i+1 }}</view>
				</scroll-view>
			</view>
			<view class="btnsWrap">
				<button class="commonBtnStyle preButton" @click="PreQuestion()">上一题</button>
				<button class="commonBtnStyle nextButton" @click="NextQuestion()">下一题</button>
				<button v-if="isEnd" @click="SubmitTest()" class="commonBtnStyle submitBtn">提交</button>
			</view>
		</view>
	</view>

</template>

<script>
	export default {
		data() {
			return {
				userName: '',
				userId: '',
				currentQuestion: 0,
				totalQuestion: 50,
				P: 0,
				A: 0,
				C: 0,
				CP: 0,
				NP: 0,
				FC: 0,
				AC: 0,
				_A: 0,
				scoreList: [
					-1, -1, -1, -1, -1,
					-1, -1, -1, -1, -1,
					-1, -1, -1, -1, -1,
					-1, -1, -1, -1, -1,
					-1, -1, -1, -1, -1,
					-1, -1, -1, -1, -1,
					-1, -1, -1, -1, -1,
					-1, -1, -1, -1, -1,
					-1, -1, -1, -1, -1,
					-1, -1, -1, -1, -1,
				],
				questions: [
					"我喜欢赞美他人",
					"不分青红皂白，会不舒服(是非明确)",
					"心软，优柔寡断",
					"性格开朗，喜欢开玩笑",
					"言谈举止稳重、冷静",
					"急性子，容易生气",
					"重视人情味",
					"好奇心强，创意性的想法也很多",
					"整理、收拾东西很在行",
					"喜欢开玩笑，调皮",
					"依赖性强",
					"能倾听别人的说话，有同理心",
					"讲义气，看到不正当的事情会挺身而出",
					"善于帮助处于困境的人",
					"说话时长使用数字和资料（data）",
					"不再看别人的眼色行事，想做什么就做什么",
					"经常后悔",
					"经常感受挫败感",
					"喜欢用（When，Where，What，Why，How）6项原则，清晰地说明问题",
					"重视业务处理的效率",
					"言谈举止缺乏自信",
					"任何事情都喜欢根据事实做判断",
					"别人求助时，很难拒绝",
					"我是讲义气，有影响力的人",
					"喜欢参加是社会公益性志愿活动",
					"同情心强，关怀他人",
					"因为高兴过度而得意忘形",
					"比看他人的优点，更容易发现别人的缺点",
					"遇到不同的意见，不容易改变自己的意见",
					"对别人温和，宽容",
					"对别人长长的说明感到不耐烦",
					"尽情的享受娱乐、酒、饮食，直到满足",
					"先做计划，后座行动",
					"一旦做了决定，就坚持行动",
					"即便是自己想做的事，别人不喜欢就会放弃",
					"喜欢运动和唱歌等休闲活动",
					"经过观察、分析后，合理地做决定",
					"得不到自己想要的东西，就忍不住",
					"觉得自己不如别人，压抑自己的感情",
					"待人宽厚、柔和，说话待人富有情感",
					"处理事情，快速而有效率",
					"面对众人说话是一件不容易的事情",
					"如我心意的人很少",
					"容易投入快乐的游戏或玩的氛围中",
					"心情脆弱，容易流泪",
					"对话常常是理性，很少用情感对话",
					"按照父母或上司的要求去做",
					"经常用【应该...做】【不可以...】",
					"常使用【哇！】【嘿!】【不得了】等赞叹词",
					"不管在任何人面前，想说什么就说什么"
				],
				isEnd: false,
				isCacuate: false,
				CP_List: [1, 5, 12, 23, 27, 30, 33, 42, 47, 49],
				NP_List: [0, 6, 11, 13, 22, 24, 25, 29, 39, 44],
				A_List: [4, 8, 14, 18, 19, 21, 32, 36, 40, 45],
				FC_List: [3, 7, 9, 15, 26, 31, 35, 37, 43, 48],
				AC_List: [2, 10, 16, 17, 20, 28, 34, 38, 41, 46]
			};
		},
		created() {
			// console.log(this.scoreList);
		},
		onLoad(options) {
			this.userName = options.userName
			this.userId = options.userId
		},
		methods: {
			AddScore(index, score) {
				const _this = this
				_this.scoreList[index] = score
				// console.log(index, score);
				// console.log(_this.scoreList[index]);
				if (_this.currentQuestion != _this.totalQuestion - 1) {
					_this.currentQuestion += 1
				} else {
					_this.isEnd = true
				}
			},
			PreQuestion() {
				const _this = this
				if (_this.currentQuestion <= 0) {
					uni.showToast({
						title: '当前是第一题',
						icon: 'none'
					})
					return;
				}

				_this.currentQuestion -= 1
				return;
			},
			NextQuestion() {
				const _this = this
				if (_this.currentQuestion >= _this.totalQuestion - 1) {
					uni.showToast({
						title: '当前是最后一题',
						icon: 'none'
					})
					return;
				}

				_this.currentQuestion += 1
				return;
			},
			SubmitTest() {
				const _this = this
				if (_this.scoreList.includes(-1)) {
					uni.showToast({
						title: '还有未完成',
						icon: 'error'
					})
				} else {
					uni.showLoading({
						title: '正在计算分数'
					})
					const isCalculated = _this.CalculateResult()
					if (isCalculated) {
						uni.hideLoading()
						const resultScore = JSON.stringify({
							'A': _this.A,
							'P': _this.P,
							'C': _this.C,
							'CP': _this.CP,
							'NP': _this.NP,
							'FC': _this.FC,
							'AC': _this.AC,
							'_A': _this._A
						})

						uni.navigateTo({
							url: "/pages/resultPage/resultPage?resultScore=" + resultScore + "&username=" + _this.userName
							+ "&userid=" + _this.userId
						})
					}
				}
			},
			GoToQuestionByIndex(index) {
				const _this = this
				_this.currentQuestion = index
			},
			CalculateSingleResult(indexList) {
				var result = 0
				for (var i = 0; i < indexList.length; i++) {
					result += this.scoreList[indexList[i]]
				}
				return result
			},
			CalculateResult() {
				const _this = this
				_this.CP = _this.CalculateSingleResult(_this.CP_List)
				_this.NP = _this.CalculateSingleResult(_this.NP_List)
				_this._A = _this.CalculateSingleResult(_this.A_List)
				_this.FC = _this.CalculateSingleResult(_this.FC_List)
				_this.AC = _this.CalculateSingleResult(_this.AC_List)
			console.log(_this._A);
				_this.P = _this.CP + _this.NP
				_this.A = _this._A * 2
				_this.C = _this.FC + _this.AC
				return true
			},
			
		}
	}
</script>

<style lang="scss">
	.testWarp {
		display: flex;
		height: 100vh;
		flex-direction: column;
		justify-items: center;
		justify-content: space-between;
		align-items: center;
		overflow: hidden;
	}

	.indexWarp {
		width: 80vw;
		margin-top: 60rpx;

		.currentIndex {
			font-weight: 1000;
			font-size: 150rpx;
			color: #ffa631;
		}

		.totalNum {
			font-size: 50rpx;
			font-weight: 800;
			color: #f08c1f;
		}
	}

	.questionWarp {
		width: 80vw;
		height: 180rpx;
		font-size: 35rpx;
	}

	.scoreWarp {
		display: flex;
		justify-content: space-between;
		width: 80vw;

		.noSekected {
			display: inline-block;
			width: 65rpx;
			height: 65rpx;
			text-align: center;
			line-height: 65rpx;
			border-radius: 50rpx;
			border: 1px solid #ffa631;
			color: #ffa631;
		}

		.scoreItem {
			display: inline-block;
			width: 65rpx;
			height: 65rpx;
			text-align: center;
			line-height: 65rpx;
			border-radius: 50rpx;
			border: 1px solid #ffa631;
			color: #fff;
			background-color: rgba(255, 166, 49, 0.8);
		}
	}

	.testTitA {
		width: 80vw;
		color: #333;
		font-weight: 800;
	}

	.testTit {
		width: 80vw;
		color: #666;
		font-size: 20rpx;
		margin-bottom: 30rpx;
	}

	.boardWarp {
		margin-top: 60rpx;
		margin-bottom: 100rpx;
		height: 300rpx;

		.boardTit {
			color: #ffa631;
			font-weight: 800;
			font-size: 40rpx;
		}

		.boardScorll {
			margin-top: 20rpx;
			width: 80vw;
			height: 300rpx;
			display: flex;
			flex-direction: row;
			flex-wrap: wrap;
			justify-content: space-between;
		}

		.finishedItem {
			text-align: center;
			line-height: 70rpx;
			display: inline-block;
			margin-right: 20rpx;
			margin-bottom: 20rpx;
			width: 70rpx;
			height: 70rpx;
			border-radius: 15rpx;
			color: #fff;
			font-weight: 800;
			background-color: rgba(255, 166, 49, 0.4);
			border: 1px solid #f08c1f;
		}

		.boardItem {
			text-align: center;
			line-height: 70rpx;
			display: inline-block;
			margin-right: 20rpx;
			margin-bottom: 20rpx;
			width: 70rpx;
			height: 70rpx;
			border-radius: 15rpx;
			color: #ffa631;
			font-weight: 800;
			border: 1px solid #ffa631;
		}
	}

	.btnsWrap {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		margin-bottom: 30rpx;
		width: 80vw;

		.preButton {
			flex: 1;
			margin-right: 10rpx;
		}

		.nextButton {
			flex: 1;
			margin-left: 10rpx;
		}

		.submitBtn {
			flex: 1;
			margin-left: 20rpx;
		}

		.commonBtnStyle {
			background-color: #ffa631;
			color: #fff;
			border: 1px solid #f08c1f;
		}


	}
</style>
