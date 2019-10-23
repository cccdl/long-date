<template>
	<view>
		<view class="long-data">
			<view class="long-data-title long-data-fl">开奖时间</view>
			<!-- <view class="long-data-fr long-data-changeTimeIcon" @tap="openStatus">></view> -->
			<view class="long-data-changeTime long-data-fr" @tap="tapIsShow">{{year}}年{{month}}月{{day}}日 {{week}} {{hour}}:{{minute}}{{ type == 'day' ? '(' + desc + ')' : ''}}
				<view v-if="showCheck" class="long-data-check">{{checkStr}}</view>
			</view>
			<!-- <view class="long-data-check-triangle"></view> -->
		</view>
		<view class="long-data-picker" :style="'display:' + [isShow ? 'block' : 'none']">
			<picker-view :indicator-style="itemHeight" :value="dateValues" @change="bindDateChange">

				<picker-view-column>
					<view class="long-datetime-item" v-for="(item,index) in dateObj.dates" :key="index">{{item}}</view>
				</picker-view-column>

				<picker-view-column>
					<view class="long-datetime-item" v-for="(item,index) in dateObj.hours" :key="index">{{item}}时</view>
				</picker-view-column>

				<picker-view-column>
					<view class="long-datetime-item" v-for="(item,index) in dateObj.minutes" :key="index">{{item}}分</view>
				</picker-view-column>

			</picker-view>
		</view>
	</view>

</template>

<script>
	export default {
		name: "long-date",
		props: {
			openStatus: { //是否展开状态
				type: Boolean,
				default () {
					return false //默认不展开
				}
			},

			type: { //模式
				type: String,
				default () {
					// return 'between' //根据开始时间和结束模式获取
					return 'day' //以天数获取开始时间模式
				}
			},
			getDayNum: { //获取天数（day模式可用）
				type: Number,
				default () {
					return 7
				}
			},
			chooesMaxDay: { //是否设置最大选择天数 （day模式可用）
				type: Number,
				default () {
					return 0 // 0 默认不设置  >0代表天数
				}
			},

			startTime: { //开始时间（between模式可用）
				type: String,
				default () {
					return ''
				}
			},

			endTime: { //结束时间（between模式可用）
				type: String,
				default () {
					return ''
				}
			},

		},



		data() {
			switch (this.type) {

				case 'between':

					//获取开始时间
					var startDate = new Date(this.startTime + ' 00:00:00')

					//获取结束时间
					var endDate = new Date(this.endTime + ' 00:00:00');

					//设置默认选择时间
					var chooesDate = new Date(this.startTime + ' 00:00:00')

					//获取多少天日期
					var num = (endDate - startDate) / 86400000

					break;

				case 'day':

					//获取当前时间
					var startDate = new Date();

					//设置默认选择时间
					var chooesDate = new Date();

					//获取多少天日期
					var num = this.getDayNum

					break;
			}

			//获取年份
			let year = chooesDate.getFullYear()

			//获取月份
			let month = chooesDate.getMonth() + 1

			//获取当前日(1-31)       
			let day = chooesDate.getDate()

			//获取当前的小时 并格式化1-10数字
			let h = chooesDate.getHours()
			let hour = h < 10 ? '0' + h : h

			//获取当前的分钟数 并格式化1-10数字
			let m = chooesDate.getMinutes()
			let minute = m < 10 ? '0' + m : m

			//设置日期数组
			let dates = []

			//设置小时数数组
			let hours = []

			//设置分钟数数组
			let minutes = []

			for (let i = 0; i <= num; i++) { //获取包括的日期

				if (i != 0) {
					startDate.setDate(startDate.getDate() + 1)
				}

				let str_year = startDate.getFullYear()
				let str_month = startDate.getMonth() * 1 + 1
				let str_day = startDate.getDate()

				if (str_month < 10) {
					str_month = '0' + str_month;
				}

				if (str_day < 10) {
					str_day = '0' + str_day;
				}

				dates.push(str_year + '-' + str_month + '-' + str_day)
			}

			for (let i = 0; i < 24; i++) {
				let str = i;
				if (i < 10) {
					str = '0' + str;
				} else {
					str = '' + str;
				}
				hours.push(str);
			}

			for (let i = 0; i < 60; i++) {
				let str = i;
				if (i < 10) {
					str = '0' + str;
				} else {
					str = '' + str;
				}
				minutes.push(str);
			}

			let dateObj = {
				dates,
				hours,
				minutes,
			}

			return {
				chooesDate,
				year,
				month,
				day,
				hour,
				minute,
				// second,
				dateObj,
				checkStr: '时间不能小于当前时间',
				showCheck: false,
				isShow: this.openStatus,
				selectRes: '',
				itemHeight: `height: ${uni.upx2px(88)}px;`,
				dateValues: [0, 0, 0]
			};
		},

		computed: {

			//获取几天后的描述
			desc: function() {
				let chooes_time = this.chooesDate
				// var chooes_time = Date.parse(new Date('2019-10-30 10:00:00')) / 1000
				let now_time = new Date()

				//获取多少秒
				let haveSecond = (chooes_time - now_time) / 86400000;

				if (haveSecond < 1) {
					return '今天'
				} else if (haveSecond > 1 && haveSecond <= 2) {
					return '明天'
				} else if (haveSecond > 2 && haveSecond <= 3) {
					return '后天'
				} else if (haveSecond > 3 && haveSecond <= 4) {
					return '3天后'
				} else if (haveSecond > 4 && haveSecond <= 5) {
					return '4天后'
				} else if (haveSecond > 5 && haveSecond <= 6) {
					return '5天后'
				} else if (haveSecond > 6 && haveSecond <= 7) {
					return '6天后'
				} else if (haveSecond > 7) {
					return '7天后'
				}
			},

			//获取星期几
			week: function() {
				let week = this.chooesDate.getDay()
				switch (week) {
					case 1:
						return '周一'
						break;
					case 2:
						return '周二'
						break;
					case 3:
						return '周三'
						break;
					case 4:
						return '周四'
						break;
					case 5:
						return '周五'
						break;
					case 6:
						return '周六'
						break;
					case 0:
						return '周日'
						break;
				}
			},
		},
		mounted() {
			this.initDate();
		},
		methods: {
			//初始化时间
			initDate() {

			},


			//切换日期选择显示状态
			tapIsShow() {
				this.isShow = !this.isShow
			},

			//滚动切换时间
			bindDateChange(e) { //有效日期的滚动日期时间方法
				let valueArr = e.detail.value;

				let dateStr = this.dateObj.dates[valueArr[0]];
				let hourStr = this.dateObj.hours[valueArr[1]];
				let minuteStr = this.dateObj.minutes[valueArr[2]];

				let chooes_time = new Date(dateStr + ' ' + hourStr + ':' + minuteStr)
				this.chooesDate = chooes_time
				this.year = chooes_time.getFullYear()
				this.month = chooes_time.getMonth() + 1
				this.day = chooes_time.getDate()

				let h = chooes_time.getHours()
				let m = chooes_time.getMinutes()
				this.hour = h < 10 ? '0' + h : h
				this.minute = m < 10 ? '0' + m : m

				let max_day = this.chooesMaxDay;
				if (max_day > 0 && this.type == 'day') {

					let haveSecond = chooes_time - new Date();


					console.log(haveSecond)
					if (haveSecond < 0) {
						this.checkStr = '时间不能小于当前时间'
						this.showCheck = true
					} else {
						
						if ((haveSecond / 86400000) > max_day) {
							this.checkStr = '时间不能大于' + max_day + '天'
							this.showCheck = true
						} else {
							this.checkStr = ''
							this.showCheck = false
						}

					}
					
					

					
				}
				this.selectRes = this.year + '-' + this.month + '-' + this.day + ' ' + this.hour + ':' + this.minute;

				this.$emit("select", {
					time: this.selectRes
				});
			}

		},

	}
</script>

<style>
	.long-data {
		margin-top: 30rpx;
		height: 80rpx;
		background: #FFFFFF;
		line-height: 80rpx;
		padding-left: 30rpx;
		padding-right: 30rpx;
		border-bottom: 1px solid #eee;
		/* position: relative; */
	}

	.long-data-check {
		height: 40rpx;
		width: 100%;
		background: #e54d42;
		position: absolute;
		left: 0;
		top: 18rpx;
		color: #fff;
		line-height: 45rpx;
		border-radius: 0rpx;
		padding: 0px 20rpx;
		font-size: 20rpx;
		text-align: center;
		border-radius: 20rpx;

	}

	.long-data-check-triangle {
		width: 0;
		height: 0;
		border-top: 12rpx solid transparent;
		border-left: 10rpx solid #e54d42;
		border-bottom: 12rpx solid transparent;
		position: absolute;
		right: 223rpx;
		top: 26rpx;


	}


	.long-data-fl {
		float: left;
	}

	.long-data-fr {
		float: right;
	}

	.long-data-changeTime {
		color: #888;
		font-size: 25rpx;
		position: relative;
		text-align: right;
		padding: 0rpx 20rpx;
	}

	.long-data-changeTimeIcon {
		color: #888;
	}

	.long-data-picker {
		width: 100%;
		height: 250rpx;
		overflow: hidden;
		background: #fff;
		transition: height 0.3s;
	}

	.long-datetime-item {
		text-align: center;
		width: 100%;
		height: 88upx;
		line-height: 88upx;
		text-overflow: ellipsis;
		white-space: nowrap;
		font-size: 30upx;
	}

	.long-data-picker picker-view {
		height: 100%;
	}
</style>
