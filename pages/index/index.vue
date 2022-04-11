<template>
	<view class="container">
		
		<uni-section title="一般用法" type="line" padding>
			<bing-countup />
		</uni-section>
		<uni-section title="不显示小时" subTitle="设置 show-hour = false 不显示小时" type="line" padding>
			<bing-countup :show-hour="false" />
		</uni-section>
		<uni-section title="文字分隔符" subTitle="设置 show-colon 属性设置分隔符样式" type="line" padding>
			<bing-countup :show-colon="false" />
		</uni-section>
		<uni-section title="修改颜色" subTitle="设置 color \ background 属性设置组件颜色" type="line" padding>
			<bing-countup color="#FFFFFF" background-color="#007AFF" />
		</uni-section>
		<uni-section title="修改字体大小" subTitle="设置 font-size 属性设置组件大小" type="line" padding>
			<bing-countup :font-size="30" />
		</uni-section>
		<uni-section title="修改颜色 + 字体大小" type="line" padding>
			<bing-countup :font-size="30" color="#FFFFFF" background-color="#007AFF" />
		</uni-section>
		<uni-section title="自由控制开始/暂停" subTitle="设置 auto-start 属性控制是否自动开启" type="line" padding>
			<bing-countup :auto-start="false" />
		</uni-section>
		<uni-section title="倒计时回调事件" type="line" padding>
			<bing-countup ref="countUp" :show-hour="false" @change="onChange" />
			<view class="" style="display: flex;">
				<button type="primary" size="mini" @click="reset">重置</button>
				<button type="primary" size="mini" @click="start">开始</button>
				<button type="primary" size="mini" @click="pause">暂停</button>
			</view>
		</uni-section>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				timeData: {},
			}
		},

		methods: {
			//开始
			start() {
				this.$refs.countUp.start();
			},
			// 暂停
			pause() {
				this.$refs.countUp.pause();
			},
			// 重置
			reset() {
				this.$refs.countUp.reset();
			},
			onChange(e) {
				this.timeData = e;
				//console.log(e);
			}
		}
	}
</script>
