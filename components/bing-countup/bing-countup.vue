<template>
	<view class="bing-countup">
		<text v-if="showHour" :style="[timeStyle]" class="bing-countup__number">{{ h }}</text>
		<text v-if="showHour" :style="[splitorStyle]"
			class="bing-countup__splitor">{{ showColon ? ':' : hourText }}</text>
		<text :style="[timeStyle]" class="bing-countup__number">{{ i }}</text>
		<text :style="[splitorStyle]" class="bing-countup__splitor">{{ showColon ? ':' : minuteText }}</text>
		<text :style="[timeStyle]" class="bing-countup__number">{{ s }}</text>
		<text v-if="!showColon" :style="[splitorStyle]" class="bing-countup__splitor">{{secondText}}</text>
	</view>
</template>

<script>
	/**
	 * Countup 数数+up=计时
	 * @description 计时器 组件
	 * 
	 * @property {Boolean} showHour = [true|false] 是否显示小时
	 * @property {Boolean} showColon = [true|false] 是否以冒号为分隔符
	 * @property {Boolean} autoStart = [true|false] 是否自动开始倒计时 （默认 true ）
	 * @property {String} backgroundColor 背景色
	 * @property {String} color 文字颜色
	 * @property {String} splitorColor 分割符号颜色
	 * @property {Number} fontSize 文字大小
	 * 
	 * @event {Function} change 倒计时变化时触发 
	 * @event {Function} start	开始倒计时
	 * @event {Function} pause	暂停倒计时 
	 * @event {Function} reset	重设倒计时，若 start 为 true，重设后会自动开始倒计时 
	 * 
	 * @example <bing-countup ></bing-countup>
	 */
	export default {
		name: 'BingCountup',
		emits: ['change', 'start', 'pause', 'reset'],
		props: {
			showHour: {
				type: Boolean,
				default: true
			},
			showColon: {
				type: Boolean,
				default: true
			},
			autoStart: {
				type: Boolean,
				default: true
			},
			backgroundColor: {
				type: String,
				default: ''
			},
			color: {
				type: String,
				default: '#333'
			},
			fontSize: {
				type: Number,
				default: 14
			},
			splitorColor: {
				type: String,
				default: '#333'
			}
		},
		data() {
			return {
				timer: null,
				syncFlag: false,
				h: '00',
				i: '00',
				s: '00',
				leftTime: 0,
				seconds: 0,
				// 倒计时是否正在进行中
				runing: false,
			}
		},
		computed: {
			hourText(val) {
				return '时'
			},
			minuteText(val) {
				return '分'
			},
			secondText(val) {
				return '秒'
			},
			timeStyle() {
				const {
					color,
					backgroundColor,
					fontSize
				} = this
				return {
					color,
					backgroundColor,
					fontSize: `${fontSize}px`,
					width: `${fontSize * 22 / 14}px`, // 按字体大小为 14px 时的比例缩放
					lineHeight: `${fontSize * 20 / 14}px`,
					borderRadius: `${fontSize * 3 / 14}px`,
				}
			},
			splitorStyle() {
				const {
					splitorColor,
					fontSize,
					backgroundColor
				} = this
				return {
					color: splitorColor,
					fontSize: `${fontSize * 12 / 14}px`,
					margin: backgroundColor ? `${fontSize * 4 / 14}px` : ''
				}
			}
		},
		watch: {
			hour(val) {
				this.changeFlag()
			},
			minute(val) {
				this.changeFlag()
			},
			second(val) {
				this.changeFlag()
			},
			autoStart: {
				immediate: true,
				handler(newVal, oldVal) {
					if (newVal) {
						this.runing = true;
						this.startData(0);
					} else {
						if (!oldVal) return
						clearInterval(this.timer)
					}
				}

			}
		},
		created: function(e) {
			this.countDown()
		},
		// #ifndef VUE3
		destroyed() {
			clearInterval(this.timer)
		},
		// #endif
		// #ifdef VUE3
		unmounted() {
			clearInterval(this.timer)
		},
		// #endif
		methods: {
			//开始
			startData(seconds) {
				console.log("计时器开始");
				if (this.seconds <= 0) {
					this.seconds = 0;
				}
				clearInterval(this.timer);
				this.seconds = seconds;
				this.runing = true;
				this.countDown();
				this.timer = setInterval(() => {
					this.seconds++
					this.countDown()
				}, 1000)
			},
			countDown() {
				let seconds = this.seconds
				let [hour, minute, second] = [0, 0, 0]
				if (seconds >= 0) {
					hour = Math.floor(seconds / (60 * 60))
					minute = Math.floor(seconds / 60) - (hour * 60)
					second = Math.floor(seconds) - (hour * 60 * 60) - (minute * 60)
				}


				/* 提交给change事件 */
				let timeData = {
					"hour": hour,
					"minute": minute,
					"second": second,
					"seconds": seconds
				}
				this.$emit('change', timeData)

				/* 规范页面显示 */
				if (hour < 10) {
					hour = '0' + hour
				}
				if (minute < 10) {
					minute = '0' + minute
				}
				if (second < 10) {
					second = '0' + second
				}
				this.h = hour
				this.i = minute
				this.s = second
			},

			changeFlag() {
				if (!this.syncFlag) {
					this.seconds = 0;
					this.startData(this.seconds);
					this.syncFlag = true;
				}
			},

			// 开始
			start() {
				console.log("开始时间");
				if (this.runing) return
				// 标识为进行中
				this.startData(this.seconds);
				this.$emit('reset')
			},

			// 重置倒计时
			reset() {
				console.log("重置时间");
				this.pause();
				this.seconds = 0;
				if (this.autoStart) {
					console.log("自动开始，从0开始");
					this.startData(0);
				}
			},
			// 暂停时间
			pause() {
				console.log("暂停时间");
				this.runing = false;
				clearInterval(this.timer);
			},
		}


	}
</script>
<style lang="scss" scoped>
	$font-size: 14px;

	.bing-countup {
		display: flex;
		flex-direction: row;
		justify-content: flex-start;
		align-items: center;

		&__splitor {
			margin: 0 2px;
			font-size: $font-size;
			color: #333;
		}

		&__number {
			border-radius: 3px;
			text-align: center;
			font-size: $font-size;
		}
	}
</style>
