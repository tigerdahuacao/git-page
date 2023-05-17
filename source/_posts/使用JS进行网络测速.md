---
title: 使用JS进行网络测速
date: 2023-04-19 17:00:19
categories:
- 开发随笔
- 教程
tags: 
- js
---

原示例在这里:
> https://animpen.com/pen/fXEk_6

怕这个个人域名过期了, 在最后把代码复制下来了

首先是第一步, 获取connection对象

![pages](使用JS进行网络测速/001.png)
在`effectiveType`

```html
<!-- 暂不支持任何预处理器 -->
<template>
	<div id="app">
		<div class="min-h-screen items-center flex justify-center p-6">
			<div class="bg-white mx-auto max-w-sm shadow-lg rounded-lg overflow-hidden border-r">
				<div class="block px-4 py-2 mb-6 leading-normal bg-grey-lighter flex flex-no-shrink">
					<h3 class="pl-2 text-center m-auto align-middle text-grey-darkest w-full">网络测速仪</h3>
				</div>
				<div class="items-center flex justify-center p-4">
					<div class="">
						<div class="speedometr">
							<div class="scale low"></div>
							<div class="scale middle"></div>
							<div class="scale hight"></div>
							<div class="arrow" :style="{ transform: 'rotate(' + calcAngle(speed) + 'deg)' }"></div>
						</div>
						<div id="counter"
							class="text-grey-darkest text-center text-base font-semibold pt-4 pb-0">{{speed.toFixed(1)}} Mbps</div>
					</div>
				</div>
				<div class="px-6 pt-0 pb-4 text-center">
					<span
						class="inline-block bg-grey-lighter rounded-full px-3 py-1 text-sm font-semibold text-grey-darker mr-2">{{type}}</span>
					<span
						class="inline-block bg-grey-lighter rounded-full px-3 py-1 text-sm font-semibold text-grey-darker mr-2"
						id="rtt">{{rtt}} ms</span>
				</div>
			</div>
		</div>
	</div>
</template>
<script>
	export default {
		data() {
			return {
				type: '-',
				angle: -75,
				rtt: 0,
				speed: 0
			};
		},
		methods: {
			getConnection() {
				const connection = navigator.connection || navigator.mozConnection || navigator.webkitConnection;
				console.log('connection', connection);
				return connection;
			},
			updateConnectionStatus() {
				this.type = this.connection.effectiveType;
				this.speed = this.connection.downlink;
				this.rtt = this.connection.rtt;
			},
			calcAngle(value) {
				var angle = -90;
				if (value < 1.5) {
					angle = (value / 1.5) * 60 - 90;
				} else if (value > 1.5 && value < 6.5) {
					angle = (value / 6.5) * 90 - 90;
				} else if (value > 6.5 && value < 15) {
					angle = (value / 15) * 30;
				} else {
					angle = (value / 15) * 90;
				}
				return Math.round(angle);
			}
		},
		mounted() {
			this.connection = this.getConnection();
			this.connection.addEventListener('change', this.updateConnectionStatus);
			this.updateConnectionStatus();
		}
	};
</script>
<style>
	@tailwind preflight;
	@tailwind components;
	@tailwind utilities;

	.speedometr {
		position: relative;
		height: 100px;
		width: 200px;
	}

	.speedometr .scale {
		position: absolute;
		border: 3px solid red;
		width: 65px;
		height: 15px;
		border-radius: 100% 100% 0 0;
		border-bottom: none;
	}

	.speedometr .scale.low {
		top: 10px;
		left: -30px;
		border-color: #e74c3c;
		transform: rotate(-50deg);
		transform-origin: top right;
	}

	.speedometr .scale.middle {
		left: calc(50% - 37px);
		border-color: #f1c40f;
	}

	.speedometr .scale.hight {
		top: 10px;
		right: -30px;
		transform: rotate(50deg);
		transform-origin: top left;
		border-color: #2ecc71;
	}

	.speedometr .arrow {
		position: absolute;
		bottom: 5px;
		left: 50%;
		margin-left: -1px;
		width: 1px;
		height: 66px;
		border: 1px solid;
		border-color: #2c3e50;
		border-radius: 100% 100% 0 0;
		background-color: black;
		transform: rotate(-75deg);
		transform-origin: bottom center;
		transition: transform 0.8s;
		transition-timing-function: cubic-bezier(0.65, 1.95, 0.03, 0.32);
	}

	.speedometr .arrow:after {
		content: '';
		display: block;
		height: 14px;
		width: 14px;
		background-color: #2c3e50;
		border-radius: 100%;
		position: absolute;
		bottom: -1px;
		left: -6px;
	}
</style>
```

