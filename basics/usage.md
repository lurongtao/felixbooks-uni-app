# 常用基本操作

> 可以尝试根据Vue使用扩展,在微信小程序,出来效果可行

重点, 统一优先Vue操作, uniApi调用

**表达式**

> 仅此而已, 与Vue使用相似, 不能使用流程控制语句

```html
<view>{{name}}</view> <!--读取变量-->

<view>{{msg+'Msea'}}</view><!--拼接变量-->
<view>{{10-9}}</view> <!--运算符-->
<view>{{0||"NB"}}</view>  <!--逻辑运算符-->

<!--数据类型方法-->
<view>{{'Msea'.indexOf('xxx')!==-1?'太原最靓仔':'no'}}</view>
<view>{{"我爱北京天安门".substr(0,2)}}</view>
```

**属性绑定**

```js
<image :src="'https://www.w3school.com.cn/i/eg_tulip.jpg'"/>
```

**动态样式**

```js
<view :class="`${'box'}`+''+`12`">{{`1`+1}}</view>  
<view :class="{className:true}"></view>
<view :class="['box',true?'col2':'']"></view>
<view class="box1212" :class="['box',true?'col2':'']"></view>
<view :style="{ color: 'red', fontSize: 20 + 'px' }">666</view>
<view :style="[{ color: pink, fontSize: 20 + 'px' }]">777</view>
```

**指令**

```html
<view class="box" v-show="true">v-show</view>

<view v-if="text==='登陆'">{{'欢迎'+name+'登陆'}}</view>
<view v-else-if="text==='未登陆'">未登陆</view>
<view v-else>!!!!!</view>

<input type="text" v-model="val">{{val}}  <!--数据双项绑定-->
<!--支持v-model修饰符-->
<input type="text" v-model.number="val">{{val+11}}
```

**事件**

```html
<!--事件,阻止事件冒泡-->
<view @click="test1(true)"> <!--传入参数-->
    <button @click.stop="test2">test</button>
</view>
```

**遍历**

```html
<!--数组遍历-->
<ul id="example-1">
  <li v-for="(item,index) in items">
    {{ item.message }}
  </li>
</ul>

<!--对象遍历-->
<div v-for="(value, name, index) in object">
  {{ index }}. {{ name }}: {{ value }}
</div>
```

### 路由跳转

**页面栈最多十层。使用uni.navigateTo频繁切换,会导致栈溢出,跳转失败**

```js
// 非tabbar配置的页面我们使用 navigateTo跳转时保留老页面,一般用于需要返回
uni.navigateTo({
    url: "../one/one?name=Msea"
})
// 跳转pages.json>tabbar>配置过的页面,使用 switchTab
uni.switchTab({
    url: "../one/one"
})
// 不保留当前页面,跳转非配置页面
uni.redirectTo({
    url: "../one/one"
})
```

**响应式单位rpx**

推荐使用最近公告推荐使用rpx替代upx

 基于苹果6    1px =2rpx

```html
<view class="box1"></view>
<view class="box2"></view>
<style lang="scss">
.box1{
	width:200rpx;
	height:200rpx;
	background:red;
}
.box2{
	width:100px;
	height:100px;
	background:pink;
}
</style>
```

### **节点操作**

微信小程序里面没有 window,document对象,那如果需要进行dom操作

```js
var query=uni.createSelectorQuery();

query.select(".titlerh").boundingClientRect((nodes)=>{
    console.log(nodes)
}).exec();

query.select(".box1")
    .boundingClientRect()
    .selectAll(".box2")
    .boundingClientRect()
    .exec((res)=>{
      console.log(res);
});
```

### 组件

创建文件夹 components目录统一写法,鼠标右键创建组件

所有组件与属性名都是小写，单词之间以连字符`-`连接。

```html
<template>
	<uni-test/>
</template>
<script>
    import uniTest from "../../components/test.vue"
	export default {
		data() {
			return {};
		},
		methods:{
			test(){
				this.text = "登录";
			}
		},
        components: {
           uniTest //引入组件
        }
	}
</script>
<style lang="scss">
    ...style
</style>
```

### 生命周期

**应用生命周期 App.vue**

```js
App({
  onLaunch (options) {
      console.log("小程序初始化");
  },
  onShow (options) 
   	  console.log("小程序显示");
  },
  onHide () {
      console.log("小程序隐藏");
  }
})
```

**分页生命周期 pages**

```js
Page({
  onLoad: function(options) {
    // 页面创建时执行
      console.log("页面加载");
  },
  onShow: function() {
   	console.log("页面进入");
  },
  onReady: function() {
    console.log("页面首次渲染完毕时执行");
  },
  onHide: function() {
    console.log("页面离开");
  },
  onPullDownRefresh: function() {
    // 触发下拉刷新时执行
    console.log("下拉触发");
    //enablePullDownRefresh 开启下拉  
  },
  onReachBottom: function() {
    // 页面触底时执行
    console.log("下拉到底");  
  },
  onShareAppMessage: function (e) {
      // 页面被用户分享时执行
      //通过按钮调用
      
      //点击触发弹窗
      <button open-type="share">分享</button>  
      console.log("用户分享");  
      return {
          title: '妹子图片',
          path: '/pages/index/index', //当前页面 path
          imageUrl: "/images/1.jpg",
          desc: '面向百度开发',
          success: (res) => {
              console.log("转发成功", res);
          },
          fail: (res) => {
              console.log("转发失败", res);
          }
      }
  },
  onPageScroll: function() {
    console.log("页面滚动时执行");
  },
 onResize: function() {
    console.log("屏幕旋转触发");
  }
})
```

**组件生命周期**

`uni-app` components组件支持的生命周期，与vue标准组件的生命周期相同。wx小程序支持生命周期

```js
beforeCreate(){} // 在实例初始化之后被调用
created(){} // 在实例创建完成后被立即调用。
beforeMount(){} // 在挂载开始之前被调用。
mounted(){} // 挂载到实例上去之后调用。
beforeDestroy(){} // 实例销毁之前调用。在这一步，实例仍然完全可用。
```

### UI组件

### uni-ui

使用方法与微信小程序一致,推荐使用uni-ui

```js
<scroll-view 
:scroll-x="true" 
style="boder:1px solid red;white-space:nowrap" 
@scroll="scroll">
        <view style="
				background:red;
				width:200px;height:100px;
				display:inline-block;"
		></view>
        <view style="background:yellow;width:200px;height:100px;display:inline-block;"></view>
        <view style="background:pink;width:200px;height:100px;display:inline-block;"></view>
        <view style="background:blue;width:200px;height:100px;display:inline-block;"></view>
</scroll-view>
```

**引入三方UI**

例如 Vant Weapp [下载](https://github.com/youzan/vant-weapp)

1. 根目录下创建 wxcomponents
2. 将下载包内部 dist包粘贴到uni-app根目录wxcomponents文件夹下
3. 配置

```js
{
    "path" : "pages/one/one",
        "style" : {
            "usingComponents":{
                "van-button": "/wxcomponents/dist/button/index" //参照wx组件引
            }
        }
}
```

4. 在App.vue,引入样式

```html
<style>
	/*每个页面公共css */
	@import "/wxcomponents/dist/common/index.wxss";
</style>
```

5. 在one分页内直接使用van-button组件,不需要引入
6. **注意如果报错,检查路径如果没有问题,可以选择重编辑器重新启动微信开发工具**

### 常用API调用

**获取用户授权弹窗**

```js
<button open-type="getUserInfo" @getuserinfo="eventName"> 获取头像昵称 </button>
getUserInfo: function(e) {
    app.globalData.userInfo = e.detail.userInfo
    this.setData({
        userInfo: e.detail.userInfo,
        hasUserInfo: true
    })
}
```

**返回所有用户授权**

```js
wx.getSetting({success:()=>{"返回用户所有用户授权"}})
```

**照相机接口**

因为,调用照相机获取临时图片格式,直接上传三方服务器,是不支持的,需要微信做解析,转发

wx.uploadFile 上传微信需要做转发看不到传输的参数

```js
uni.chooseImage({
    count: 1, //图片张数
    sizeType: ['original', 'compressed'],//原图,压缩图,
    sourceType: ['album', 'camera'],  //本地相册,拍照
    success :res=> {
        const tempFilePaths = res.tempFilePaths//微信小程序图片临时路径
        this.setData({tempFilePaths}); 
    }
})
// 预览接口
viewImgs(index) {
    uni.previewImage({
        current: this.data.tempFilePaths[index], // 当前显示图片的http链接
        urls:this.data.tempFilePaths // 需要预览的图片http链接列表
    });
},
    
//小程序图片上传    
fileUplaod() {
    uni.uploadFile({
      url: "http://wxs.ixinangou.net/index/index/dofiles",
      filePath: this.data.tempFilePaths[0],
      name: "file", //上传图片key
      formData: {
        user: "MSea" //需要额外携带参数
      },
      header: {
        "content-type": "multipart/form-data"
      },
      success: res => {
        console.log("data");
      }
    });
  }
```

**请求**

微信原生请求接口

注意设置,不效验合法域名,回忆怎么添加合法域名

```js
//GET 会自动拼接参数
//queryStringParams
uni.request({
    method: "GET",
    url: "https://cnodejs.org/api/v1/topics",
    data: { 
        uname: "Msea"
    },
    success: (res) => {
        console.log(res)
    }
})
//POST 默认参数为payLoad,为json
uni.request({
    method: "POST",
    url: "https://cnodejs.org/api/v1/topics",
    data: { 
        uname: "Msea" 
    },
    success: (res) => {
        console.log(res)
    }
})
//POST form-data 数据 
// 'content-type': 'multipart/form-data'  用于文件上传  
uni.request({
   url: 'https://cnodejs.org/api/v1/topic/5433d5e4e737cbe96dcef312',
    data:{fileId:'123'},
    method:'POST',
    header: {
        'content-type': 'application/x-www-form-urlencoded'
    },
    success:function(res){}
})
```

### 地图使用

**地图组件**

```js
<map 
    id="map" 
    longitude="经度" 
    latitude="纬度" 
    scale="缩放级别(14)" 
    bindcontroltap="点击地图触发FN" 
    markers="{{添加标记}}" 
    bindtap="markertap" 点解地图触发
    show-location  将地图中心移置当前定位点，此时需设置地图组件 show-location 为true。
    style="width: 100%; height: 300px;"></map>
```

**移动地图中心点**

​	移动端测试有效

```js
 onShow: function() {
    // 地图上下文对象
    this.mapCtx = uni.createMapContext('map');
  },
      
      ckMap(e){
          //小程序不支持 Objcet.assign
          var temp={
              iconPath: "/assets/img/local.png",
              id: 0,
              width: 25,
              height: 25,
              ...e.detail
          }
          var markers=this.data.markers;
          markers.push(temp);
          this.setData({markers},()=>{
              var data={
                  ...e.detail
              };
              this.mapCtx.moveToLocation(data)
          })
      } 
```

**定位**

```js
uni.getLocation({
    type: 'gcj02', //腾讯地图坐标系
    success:(res)=> {
        const latitude = res.latitude
        const longitude = res.longitude
        }
})  
```

**用户授权弹窗**

app.json配置文件

```js
//app.json
{
  "pages": ["pages/index/index"],
  "permission": {
    "scope.userLocation": {
      "desc": "熊猫创客需要获取您的位置亲" // 高速公路行驶持续后台定位
    }
  }
}
```

获取当前位置并移动地图中心店

```js
wx.getLocation({
    type: 'gcj02', //腾讯地图坐标系
    success: (res) => {
        const latitude = res.latitude
        const longitude = res.longitude
        var temp = {
            iconPath: "/assets/img/local.png",
            id: 0,
            width: 25,
            height: 25,
            latitude,
            longitude
        }
        var markers = this.data.markers;
        markers.push(temp);
        this.setData({
            markers
        }, () => {
            var data = {
                latitude,
                longitude
            };
            this.mapCtx.moveToLocation(data)
        })
    }
})
```

### vuex

uni-app已经内置了vuex

1. 在项目的根目录下，**创建一个名为store的文件夹**然

```js
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
const store = new Vuex.Store({
    state: {
		num:0
	},
    mutations: {
		add(store){
				store.num++;
		}
	},
    actions: {}
})
export default store
```

2. main.js入口文件引入

```js
import store from './store'
Vue.prototype.$store = store;  

const app = new Vue({
    ...App,
	store
})
app.$mount()
```

3. 组件内引入

```js
<template>
	<view>
		<view>{{num}}</view>
		<button @click="test">test1</button>
	</view>
</template>

<script>
	 import {  
	        mapState,  
	        mapMutations  
	    } from 'vuex';  
	export default {
		computed:{
			...mapState(['num'])
		},
		methods:{
			test() {
				console.log(this.$store.commit("add"))
			}
		}
	}
</script>
```

### 条件编译

可以安装支付宝小程序进行测试

```java
 #ifdef %PLATFORM% 
    //这些代码只在该平台编译
 #endif
 
#ifdef ：      if defined  仅在某个平台编译
#ifndef ：     if not defined  在除里该平台的其他编译
#endif ：      end if 结束条件编译
%PLATFORM%     需要编译的平台，上面的MP就是各个小程序的意思

常用 %PLATFORM% 
h5  h5平台
MP-WEIXIN  微信小程序
MP-ALIPAY  支付宝小程序
MP-BAIDU   百度小城
MP-TOUTIAO 头条小程序
```

总结: 优先使用Vue用法,只要实现微信开发,支持wx,ui,api,统一使用uni的Api,多看文档