## 可直接拖进项目运行

## QQ交流群: 750104037 [点我加入](https://jq.qq.com/?_wv=1027&k=5OyZoXa)

---
## 作者想说
二维码生成参考了 `诗小柒` 的二维码生成器， 感谢 `诗小柒`!
也感谢qq：1846492969 的帮助

如果该插件有什么问题还请大家说出来哦，还有如果有什么建议的话也可以提下呐 ~
如果觉得好用，可以回来给个五星好评么~~(❁´◡`❁)\*✲ﾟ\*  蟹蟹~拜托啦~

---
## 组件简介
一款自我感觉良好、实用的海报生成器

---

# 警告
`小程序中请注意自己的获取图片信息的api--getImageInfo 是否正常获取图片信息，否则绘制不出来`
`重要:`如果小程序有绘制微信用户头像需求的, 把https://thirdwx.qlogo.cn、https://wx.qlogo.cn都配置到小程序后台下载域名去

## `目前请使用网络图片!`

# 兼容性
### APP、微信小程序、H5

#  1. 相关说明
## 1.0.0画布相关
```
首先目前有两种方式, 根据背景图绘制和自定义画布绘制,
1、根据背景图绘制
	该方式中画布大小跟随背景图大小，而背景图的设置有两种方式:
		a、app.js中的getPosterUrl方法获取 (若采用此方法则需自行配置)
		b、传参backgroundImage为图片地址
2、自定义画布绘制
	该方式需传background属性, 根据其属性控制画布的大小颜色

优先级: background> backgroundImage> app.js-getSharePoster()
注: 所以如果传了backgroundImage就不会去app.js获取背景图片, 如果传了background就不会绘制背景图片
```
## 1.0.1布局相关
```
因为ios的原因会将画布大小进行默认0.75的缩放(目前不知道原因), 可以手动设置bgScale参数控制比例
故绘制时各项参数若不是通过bgObj(背景对象, 包含画布宽高等参数) 动态控制的 则应该乘以bgScale参数
```
## 1.0.2其他
```
注意：其实该插件主要使用的其中的js， 至于图片展示问题其实是开发者自行实现的

使用步骤
1、在需使用的vue页面中`创建一个canvas`，并自定义id，该id需传入js方法中，并`动态绑定宽高`
2、引入QS-SharePoster.js，该js文件导出一个对象,使用该对象中的getSharePoster方法
3、使用getSharePoster方法。 该方法`接收一个对象`，并返回一个`Promise对象`，在页面中可以使用async、await的方式，也可以用then方法, `该方法最终返回绘制好的海报临时路径`

注意: 若没有自行控制画布宽高，则应该设置setCanvasWH方法并设置画布宽高

```

# 2. 传入参数详解

`注：所有传入的图片路径都为网络图片`

| 属性名 | 是否必填 | 值类型 | 默认值 | 说明 |
| --------- | -------- | -----: | --: | --: |
| posterCanvasId | 是 |  String |  | 页面中绘制海报的画布的id |
| type |   | all|    |   自定义标识，用于逻辑判断 |
| backgroundImage |  | String |  | 背景图片路径，优先级大于app.js中getPosterUrl方法返回的路径 |
| reserve| | Boolean | false | 本次绘制是否接着上一次绘制 |
| qrCodeArray| | Array \| Function| | 需绘制的二维码数组，若传入的类型为Function，则可以接收一个对象, 该对象有三个参数，bgObj是背景图片的宽高等信息，type是自定义标识type，bgScale是背景图片宽高缩放比例, 且必须return一个数组，推荐传入Function类型, `详见2.0.1`|
| imagesArray| | Array \| Function | |需绘制的图片数组，若传入的类型为Function，则可以接收一个对象, 该对象有三个参数，bgObj是背景图片的宽高等信息，type是自定义标识type，bgScale是背景图片宽高缩放比例, 且必须return一个数组，推荐传入Function类型,`详见2.0.2` |
| setCanvasWH| （一般来说）是| Function | | 动态设置画布宽高的方法，该方法返回一个对象, 该对象有三个参数，bgObj是背景图片的宽高等信息，type是自定义标识type，bgScale是背景图片宽高缩放比例, 可以根据背景图给画布定宽高，`详见2.0.3` |
| setCanvasToTempFilePath| | Object\|Function  | | 设置绘制完毕后的生成海报临时路径参数,该参数有默认, 一般可以不用填, `详见 2.0.4`|
| setDraw| | Function | | 自定义绘制方法，该方法接收一个对象, `详见 2.0.5` |
| background| | Object| | 背景图对象, `详见 2.0.6` |
| textArray| |  Array \| Function| | 需绘制的文本数组，若传入的类型为Function，则可以接收两个参数，第一个参数是背景图片的宽高等信息，第二个参数为自定义标识type ， 且必须return一个数组，推荐传入Function类型, `详见 2.0.7` |
| bgScale(v14.0新增)| |  Number | 0.75| 为了IOS能正常绘制对背景图进行缩放，取值(0,1)， 若IOS还是绘制不出可以 尝试下调此参数 |
| Context(v17.0新增)| |  CanvasContext| | 画布对象，一般不用 |
| `v3.0.0更新` | |  | |   `v3.0.0开始推荐使用drawArray绘制, 不需要再用其他的xxxArray绘制了`|
| drawArray| | Array \| Function| | 可控层级的绘制序列, 层级的顺序就是数组的顺序, 可直接return数组，也可以return一个promise对象, 但最终resolve一个数组, 这样就可以方便实现后台可控绘制海报, 详见[2.0.8](#drawArray) |
| delayTimeScale| | Number| 15| 根据绘制序列每一项延时该参数的时间 |
| drawDelayTime| | Number| 100| draw方法延时时间|
| _this| | Vue | | 传入当前vue实例this指向, 组件中使用时必传|
| `v3.0.1更新` | |  | |   |
| formData| | Any | | app.js中的getPosterUrl方法可获得该属性 用于获取背景图时携带的自定义数据|

## 2.0.8 drawArray参数详解<span id="drawArray" />
```
该参数可以是一个数组，也可以是一个方法, `若传入一个方法，则必须return一个数组`， 也允许返回promise对象
数组中的项内属性: 
```

| 属性名 | 是否必填 | 值类型 | 默认值 | 说明 |
| --------- | -------- | -----: | --: | --: |
| `v3.0.1更新` | |  | |   |
| allInfoCallback | | Function|  | 该参数传入一个方法，该方法接收一个对象，该对象目前包含一个drawArray属性, 可以获取全部的drawArray的详细参数, 该方法可以return一个对象或promise对象，最终输出一个对象, 该对象的属性将会覆盖原属性|
| serialNum| | Number|  -∞| 用于控制序列中allInfoCallback的顺序, 数值越小, 越先获取|
| `over` | |  | |   |
| type| 是| String|  | 绘制类型, 可选值 'text'、'image'、'qrcode'、'custom'|
| `type为text时`| | |  |  |
|... | | |  | 参数详见[`textArray`](#textArray) |
| `type为image时`| | |  |  |
|... | | |  | 参数详见[`imagesArray`](#imagesArray) |
| `type为qrcode时`| | |  |  |
|... | | |  | 参数详见[`qrcodeArray`](#qrcodeArray) |
| `type为custom时`| | |  |  |
| setDraw| | |  | 该属性传入一个方法，接收一个参数-画布对象，可在方法中自定义绘制内容|

注: 当drawArray为function时，v3.0.5新增setBgObj与getBgObj方法，详见示例项目

## 2.0.1 qrCodeArray参数详解 <span id="qrcodeArray" />
```
该参数可以是一个数组，也可以是一个方法, `若传入一个方法，则必须return一个数组`
数组中的项内属性: (基本与诗小柒的二维码生成器传入参数差不多，就是多了dx、dy)
```

| 属性名 | 是否必填 | 值类型 | 默认值 | 说明 |
| --------- | -------- | -----: | --: | --: |
| text| 是 |  String |  | 二维码内容 |
| size|   | Number|  |  二维码大小 |
| dx|   | Number|  |  二维码在x轴上的位置 |
| dy|   | Number|  |  二维码在y轴上的位置 |
| background|   | String |  |  二维码背景色 |
| foreground|   | String |  |  二维码前景色 |
| correctLevel|   | Number |  |  二维码容错级别 |
| image|   | String |  |  二维码中心的图片 |
| imageSize|   | Number |  |   二维码图标大小 |

传入为Function类型示例代码: (推荐使用Function类型)
```javascript
qrCodeArray: ({bgObj, type, bgScale}) => {
								return [{
									text: '你好，我是取舍',
									// #ifndef H5
									image: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1559644434957&di=0db394a4ae41b6cff704fa3d4cbd997b&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201806%2F30%2F20180630233629_GueV4.thumb.700_0.jpeg',
									// #endif
									// #ifdef H5
									image: '/static/2.jpg',
									// #endif
									size: bgObj.width*0.2,
									dx: bgObj.width*0.05,
									dy: bgObj.height - bgObj.width*0.25
								}]
							}
```


## 2.0.2 imagesArray参数详解 <span id="imagesArray" />
```
该参数可以是一个数组，也可以是一个方法, `若传入一个方法，则必须return一个数组`
数组中的项内属性: (与官方文档中的drawImage方法传入的参数一致，一般最多填前5个参数)
```

| 属性名 | 是否必填 | 值类型 | 默认值 | 说明 |
| --------- | -------- | -----: | --: | --: |
| url| 是 |  String |  | 网络图片路径 |
| dx|   | Number|  |  图像的左上角在目标canvas上 X 轴的位置 |
| dy|   | Number|  |  图像的左上角在目标canvas上 Y 轴的位置 |
| dWidth|   | Number|  |  在目标画布上绘制图像的宽度，允许对绘制的图像进行缩放 |
| dHeight|   | Number|  |  在目标画布上绘制图像的高度，允许对绘制的图像进行缩放 |
| sx|   | Number|  |  源图像的矩形选择框的左上角 X 坐标 |
| sy|   | Number|  |  源图像的矩形选择框的左上角 Y 坐标 |
| sWidth|   | Number|  |   源图像的矩形选择框的高度 |
| sHeight|   | Number|  |   源图像的矩形选择框的高度 |
| circleSet(5.0新增)|   | Object\|Boolean |  |   图片圆形设置, 详见`2.0.2.0.3` |
| infoCallBack (4.0新增)|   | Function|  |  接收自身图片信息并返回新参数的回调函数, 详见`2.0.2.0.2`示例 |
| roundRectSet(v16.0新增)| |  Object\|Boolean | | 圆角矩形图片设置, 详见`2.0.2.0.4` |
| `v3.0.1 新增`| | | | |
| alpha| |  Number | 1| 图片的透明度, 取值: [0, 1] |

### 2.0.2.0.1传入为Function类型示例代码(无infoCallBack ): (推荐使用Function类型)
```javascript
imagesArray: ({bgObj, type, bgScale}) => { //接收的第一个参数为背景图片的信息, 第二个参数是自定义标识）, 图片为示例图片
	return [{
		url: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1559722952847&di=ce05a7206d3e0adeb1909a70ff7410ae&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201708%2F16%2F20170816080523_TmCUK.thumb.700_0.jpeg',
		dx: 300,
		dy: bgObj.height - 300,
		dWidth: 200,
		dHeight: 200
}]
}
```

### 2.0.2.0.2传入为Function类型示例代码(含infoCallBack ): (推荐使用Function类型)
```javascript
imagesArray: ({bgObj, type, bgScale}) => { //接收的第一个参数为背景图片的信息, 第二个参数是自定义标识（感觉这里用不到）, 图片为示例图片
  return [{
  	url: 'https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=1314428097,3858988978&fm=26&gp=0.jpg',
  	dx: 300,
  	dy: bgObj.height - 300,
  	infoCallBack(imageInfo) { //接受自身的图片信息对象
  		let scale = 200/imageInfo.height;
  		return {  // 需return新参数， 会覆盖原先的参数
  			dWidth: imageInfo.width*scale,
  			dHeight: 200
  		}
  	}
  }]
}
```

### 2.0.2.0.3 circleSet属性详解
| 属性名 | 是否必填 | 值类型 | 默认值 | 说明 |
| --------- | -------- | -----: | --: | --: |
| ~~circle~~ `废弃`|  |  Boolean|  | 是否设置圆形 |
| r|  |  Number| 详见下方2.0.2.0.3.0.1 | 圆形半径 |
| x|  |  Number| 详见下方2.0.2.0.3.0.1 | 圆形x坐标 |
| y|  |  Number| 详见下方2.0.2.0.3.0.1 | 圆形y坐标 |

注： r、x、y的值最终会传至Context.arc()方法, 并都有默认值，详见下方2.0.2.0.3.0.1

### 2.0.2.0.3.0.1 r、x、y默认值详解
r的默认值先判断是否有dWidth和dHeight,若有则取两者之间值较小的, 若无，则判断img自身的宽高取两者之间较小的，最终除以2<br />x的默认值为(dx  || 0 + r)<br />y的默认值为(dy || 0 + r)

### 2.0.2.0.4 roundRectSet属性详解
| 属性名 | 是否必填 | 值类型 | 默认值 | 说明 |
| --------- | -------- | -----: | --: | --: |
| r|  |  Number| 输出的图片宽度*.1 | 圆角半径 |


## 2.0.3 setCanvasWH参数详解
```
该参数传入一个方法，该方法返回两个参数，接收的第一个参数为背景图片的信息, 第二个参数是自定义标识）
注： 确保能够改变画布大小，不然会有问题
```

示例代码:
```javascript
setCanvasWH: ({bgObj, type, bgScale}) => { // 为动态设置画布宽高的方法，
	this.poster = bgObj;
},
```


## 2.0.4 setCanvasToTempFilePath参数详解
```
该参数传入一个方法，该方法返回两个参数，接收的第一个参数为背景图片的信息, 第二个参数是自定义标识）
该参数有默认， 一般可以不用填
必须return一个对象, 格式: (与官方文档中的setCanvasToTempFilePath方法传入的参数基本一致，除了canvasid与回调函数不用传)
```

| 属性名 | 是否必填 | 值类型 | 默认值 | 说明 |
| --------- | -------- | -----: | --: | --: |
| x| |  Number |  | 画布x轴起点（默认0） |
| y|   | Number|  |  画布y轴起点（默认0） |
| width|   | Number|  |  画布宽度（默认为canvas宽度-x）|
| height|   | Number|  |  画布高度（默认为canvas高度-y） |
| destWidth|   | Number|  | 输出图片宽度（默认为 width * 屏幕像素密度） |
| destHeight|   | Number|  |  输出图片高度（默认为 height * 屏幕像素密度） |
| fileType|   | String|  jpg|  目标文件的类型，只支持 'jpg' 或 'png' |
| quality|   | Number| .8 |   图片的质量，取值范围为 (0, 1]，不在范围内时当作1.0处理 |

注：v11.0版本后该参数为拓展覆盖，所以无需所有参数都传

## 2.0.5 setDraw参数详解
```
该参数传入一个方法，该方法返回一个对象，该对象有三个参数{Context,bgObj,type}，分别是绘图上下文、背景图片的信息、自定义标识，可以在此方法中绘制自定义内容
示例代码:
```

```javascript
setDraw: ({Context, bgObj, type, bgScale}) => {
		Context.setFillStyle('black');
		Context.setGlobalAlpha(0.3);
		Context.fillRect(0, bgObj.height - 400, bgObj.width, 400);
		Context.setGlobalAlpha(1);
		Context.setFillStyle('white');
		Context.setFontSize(50);
		let text = '取舍'
		Context.fillText(text, bgObj.width - text.length * 50 - 50, bgObj.height - 185);
}
```

## 2.0.6 background参数详解
```
该参数传入一个对象，若无背景为图片的需求可用此属性设置画布背景的大小、颜色等, 若传该属性， 则不会去获取背景图片了
```

| 属性名 | 是否必填 | 值类型 | 默认值 | 说明 |
| --------- | -------- | -----: | --: | --: |
| width|   | Number|  |  背景宽度|
| height|   | Number|  |  背景高度|
| backgroundColor| | Color| black  | 背景颜色 |


## 2.0.7 textArray参数详解 <span id="textArray" />
```
该参数可以是一个数组，也可以是一个方法, `若传入一个方法，则必须return一个数组`
数组中的项内属性: 
```

| 属性名 | 是否必填 | 值类型 | 默认值 | 说明 |
| --------- | -------- | -----: | --: | --: |
| text| 是| String|  |  需绘制的文本|
| size|   | Number| 50 |  文字大小，v18.0+ `设置的文字若携带小数， 会向上取整`|
| color| | Color| black | 文字颜色 |
| alpha| | Number| 1| 透明度, 取值[0, 1] |
| textAlign| | String| left| 文字的x轴对齐模式, 可选值: 'left'、'center'、'right' |
| textBaseline| | String| middle| 文字的y轴对齐模式, 可选值: 'top'、'bottom'、'middle'、'normal' |
| dx| | Number| 0| 文字x轴位置 |
| dy| | Number| 0| 文字y轴位置 |
| lineThrough| | Object| | 设置删除线, `详见2.0.7.0.1`|
| infoCallBack| | Function| | 携带文本长度值的方法, 可以return一个对象，内含自身属性，并覆盖|
| lineFeed(v13.0新增)| | Object| | 设置换行, `详见2.0.7.0.2`|
|`v19.0新增`|          |        |     |     |
| font |  | String | 10px sans-serif | 字体属性, 若填此参数将忽略下方四个参数, 注意 字体大小需为整数 |
| fontStyle | | String | normal | 字体样式, 详见2.0.3 |
| fontVariant| | String | normal | 字体异体, 详见2.0.4 |
| fontWeight| | String | normal | 字体粗细, 详见2.0.5 |
| fontFamily| | String | sans-serif| 字体系列 |

### 2.0.7.0.1 lineThrough参数详解
```
用于设置删除线
传入参数如下:
```

| 属性名 | 是否必填 | 值类型 | 默认值 | 说明 |
| --------- | -------- | -----: | --: | --: |
| width|   | Number| (`2.0.7`中设置的size)/10 |  删除线宽度|
| style| | Color| `2.0.7`中设置的color| 删除线颜色 |
| alpha| | Number| `2.0.7`中设置的alpha| 透明度, 取值[0, 1] |
| cap| | String| butt| 设置线条的端点样式 |

```javascript
textArray: ({bgObj, type, bgScale}) => {
	return [{
		text: '取舍',
		size: 50,
		color: 'white',
		alpha: .5,
		textAlign: 'left',
		textBaseline: 'middle',
		lineThrough: {
		  style: 'red'
		},
		infoCallBack(textLength) {
			return {
				dx: bgObj.width - textLength - 50,
				dy: bgObj.height - 245
			}
		}
	}]
}
```

### 2.0.7.0.2 lineFeed参数详解
```
用于设置换行
传入参数如下:
```

| 属性名 | 是否必填 | 值类型 | 默认值 | 说明 |
| --------- | -------- | -----: | --: | --: |
| maxWidth|   | Number| 画布宽度 |  设置单行最大宽度|
| lineHeight| | Number| `2.0.7`中设置的size|行高， 一般要大于设置的字体大小 |
| lineNum| | Number| -1| 最多行数，若小于零则为无限 |
| dx| | Number| `2.0.7`中设置的dx| 换行后的x轴位置，不包含第一行，若小于零则为`2.0.7`中设置的dx |

```javascript
textArray: ({bgObj, type, bgScale}) => {
  return [{
  	text: '取舍取舍取舍取舍取舍取舍',
  	size: 50,
  	color: 'red',
  	alpha: .5,
  	textAlign: 'left',
  	textBaseline: 'middle',
  	lineFeed: {
  		maxWidth: 150,
  		lineHeight: 50,
  		lineNum: -1,
  		dx: -1
  	},
  	infoCallBack(textLength) {
  		return {
  			dx: 200,
  			dy: 20
  		}
  	}
  }]
}
```

### 2.0.7.0.3 fontStyle 参数详解
| 属性名 |  说明 |
| --------- | ---- |
| normal | 默认值。标准的字体样式 |
| italic | 斜体的字体样式 |
| oblique | 倾斜的字体样式 |

### 2.0.7.0.4 fontVariant参数详解
| 属性名 |  说明 |
| --------- | ---- |
| normal | 默认值。标准的字体 |
| small-caps | 小型大写字母的字体 |

### 2.0.7.0.5 fontWeight参数详解
| 属性名 |  说明 |
| --------- | ---- |
| normal | 默认值。定义标准的字符。 |
| bold | 定义粗体字符。 |