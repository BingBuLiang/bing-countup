# bing-countup

一个轻量级计时器插件

### 安装方式

本组件符合[easycom](https://uniapp.dcloud.io/collocation/pages?id=easycom)规范，`HBuilderX 2.5.5`起，只需将本组件导入项目，在页面`template`中即可直接使用，无需在页面中`import`和注册`components`。

### 示例

在 ``template`` 中使用组件

```vue
<template>
	<view class="container">
        <!--  一般用法 -->
		<bing-countup />
        <!-- 不显示小时 设置 show-hour = false 不显示小时-->
		<bing-countup :show-hour="false" :hour="12" :minute="12" :second="12" />
        <!-- 文字分隔符 设置 show-colon 属性设置分隔符样式-->
		<bing-countup :show-colon="false" />
        <!-- 修改颜色 设置 color \ background 属性设置组件颜色-->
		<bing-countup color="#FFFFFF" background-color="#007AFF"/>
        <!-- 修改字体大小 设置 font-size 属性设置组件大小-->
		<bing-countup :font-size="30" />
        <!-- 修改颜色 + 字体大小 -->
		<bing-countup :font-size="30" color="#FFFFFF" background-color="#007AFF" />
        <!-- 自由控制开始/暂停 设置 auto-start 属性控制是否自动开启-->
		<bing-countup :auto-start="false" />
        <!-- 倒计时回调事件 -->
		<bing-countup ref="countUp" :show-hour="false" @change="onChange" />
        <view class="" style="display: flex;">
            <button type="primary" size="mini" @click="reset">重置</button>
            <button type="primary" size="mini" @click="start">开始</button>
            <button type="primary" size="mini" @click="pause">暂停</button>
    	</view>
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

```
#### 如果需要引用，可以使用以下方式
```javascript
<script>
	import BingCountup from "@/components/bing-countup/bing-countup.vue"
	export default {
		components: {
			'bing-countup': BingCountup
		}
    }
</script>
```


## API

### bing-countup @property

| 属性名          | 类型    | 默认值 | 说明                        |
| --------------- | ------- | ------ | --------------------------- |
| showHour        | Boolean | true   | 是否显示小时，默认是        |
| showColon       | Boolean | true   | 是否以冒号为分隔符，默认是  |
| autoStart       | Boolean | true   | 是否自动开始倒计时 ，默认是 |
| backgroundColor | String  | ' '    | 背景色                      |
| color           | String  | '#333' | 文字颜色                    |
| fontSize        | Number  | 14     | 文字大小                    |
| splitorColor    | String  | '#333' | 分割符号颜色                |

### bing-countup @event

| 事件名  | 说明                     | 返回值                                                       |
| ------- | ------------------------ | ------------------------------------------------------------ |
| @change | 倒计时变化时触发         | Object<br/> {<br/>					"hour": hour,<br/>					"minute": minute,<br/>					"second": second,<br/>					"seconds": seconds<br/>} |
| @start  | 开始倒计时(refs方式调用) | 无                                                           |
| @pause  | 暂停倒计时(refs方式调用) | 无                                                           |
| @reset  | 重置倒计时(refs方式调用) | 无                                                           |

## 鸣谢

> 开发做题系统，没有一款满意的计时器组件，一般来说，都是倒计时组件，本来想引用smh-timer这个组件，但是不显示小时，在此和uni-countdown基础上开发出来的，有什么问题希望大家留言~，若有时间，会整合一下，倒计时，这样倒计时和计时器一个组件就可以了