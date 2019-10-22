# 欢迎使用 long-date 嵌入式时间选择插件

>*long-date 是一个嵌入式时间选择插件，提供两种时间选择方式*


组件名： `long-date`

### 版本预告
1. 加上展开过渡动画；增加弹框模式
2. day模式 精确到当前小时数和分钟数，当页面打开后停止一段时间，还是可以选择已过去的时间
3. 不满足验证条件则返回错误信息
4. 自定义格式化返回固定时间
5. 增加多种模式，例如 区间时间；酒店价格时间模式；
6. 目前没有精确到秒，看反馈怎么样，需要的话加上


>*特别说明：因为作者是根据自己的项目开发的插件，并且适用自己的项目，版本更新可能较慢，如需要其他功能，可在文件基础上二次开发，如果需要定制可以联系作者，邮箱：568078220@qq.com*



#### 模式

- 根据开始时间和结束时间【between模式】
- 根据开始时间和未来天数【day模式】

#### 调用方式：
```html
<long-date 
	type="between" 
	getDayNum="10" 
	:openStatus="true" 
	startTime="2018-10-21" 
	endTime="2019-06-01" 
	chooesMaxDay="8"
	@select="onSelectTime">
</long-date>
```

#### js:

```javascript
import longDate from "@/components/long-date/long-date.vue"
export default {
    components: {
        longDate
    },
    data() {
        return {};
    },
    methods: {
        onSelectTime(val) {
			console.log(val);
		}
    }
};
```

#### 属性说明：
属性名  | 类型  | 默认值 | 是否必填 | 说明
 ---- | ----- | ------ | ------ | ------
type  | String  | day | 否 | day：未来天数模式；between：区间模式
getDayNum  | Number  | 7 | 否 | 未来天数模式，设置后展示当前时间的未来默认7天的时间选项
chooesMaxDay  | Number  | 0 | 否 | 未来天数模式，设置后，验证选择时间是否大于当前时间chooesMaxDay天，0为不验证; >0则验证大于chooesMaxDay天
openStatus  | Boolean  | false | 否 | 是否展开picker-view滚动选择器
startTime  | String  | 2018-10-01 | 否 | 区间模式；必填，开始时间
endTime  | String  | 2019-01-01 | 否 | 区间模式；必填，结束时间



#### 方法说明：
时间名  | 类型  | 说明 
 ---- | ----- | ------
select  | function  | 滚动返回时间
1

#### 事件返回说明：
```json
{time: "2019-10-26 00:00"}
```

