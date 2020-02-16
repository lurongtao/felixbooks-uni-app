# uni-app (一套代码,多端发行)

**如果你会使用Vue那咱们继续吧!**

### **什么是uni-app**?

uni-app 是一个使用 [**Vue.js**](https://vuejs.org/) 开发所有前端应用的框架，开发者编写一套代码，可发布到iOS、Android、H5、以及各种小程序（微信/支付宝/百度/头条/QQ/钉钉）等多个平台。

### **诞生背景**

多端泛滥 iOS、Android、H5、以及各种小程序（微信/支付宝/百度/头条/QQ/钉钉)多套平台,多套文档,加大开发维护成本

### **uni-app特点**

1. 跨平台

   一套代码多端发行,而不失优雅(条件编译,保留不同平台独有特色功能方法调用)

   ```js
   <!--#ifdef MP-WEIXIN-->
   	<view>只会编译小程序</view>
   <!--endif-->
   
   <!--#ifdef XX-XXX-->
   	<view>只会编译XXX</view>
   <!--endif-->        
   ```

2. 通用技术栈,学习成本低 

   Vue的语法,微信小程序Api,内嵌mpvue可以直接迁移,如果你会Vue可以直接入门

3. 生态丰富

   支持npm下载第三方模块,支持小程序自定义组件,SDK,兼容mpvue组件,支持原生混合编码

### 安装

​	可以下载两个进行配置 ,测试微信小程序与支付宝小程序同步实现

​	编译工具下载[HBuilderX](https://www.dcloud.io/)

​	微信小程序开发[IDE](https://developers.weixin.qq.com/miniprogram/dev/devtools/devtools.html)

​ 支付宝小程序 [IDE](https://render.alipay.com/p/f/fd-jwq8nu2a/pages/home/index.html)

​**推荐使用 HBuildX 进行开发**

### 跨多端小程序开发

**创建目录**

```
1. 打开HBuilderX
```

2. 左上角创建 项目
1. 选择uni-app项目
3. 下面是提供几个Demo项目,可以创建学习
4. 项目名称能不能大写,创建项目

**运行项目**

1. 找到创建项目uni里面的项目目录
2. 找到 manifest.js文件输入
   1. 里面有所有平台配置文件
      1. 需要不通平台测试,需要分别配置不同平台
   2. 选择微信小程序配置
      1. 配置小程序AppID
      2. 点击最上面工具栏选择运行
      3. 选择运行到小程序模拟器
      4. 选择微信小程序开发工具
         1. 第一次配置小程序开发者工具,需要打开服务
3. 打开微信小程序开发左上角小
        3. 安全设置，将服务端口开启。||工具 -> 设置 -> 安全设置，将服务端口开启。
4. 选择HBuild停止服务,重新运行自动打开编辑工具

.nvue 是对weex的增强。如果你不开发App，那么你不太需要nvue。

**发布测试**

选择HBuild最上面工具栏,点击发布进行打包,根据命令行提示在点击微信开发者工具发布

**注意不要直接修改微信开发工具目录,以HBuild开发目录为主**

**目录介绍**

与小程序配置相似

```js
pages //存放分页目录
static //存放应用引用静态资源
main.js //入口文件
mainfest.json //跨平台所有配置项文件
pages.json //全局配置文件,类似App.json 
uni.scss//全局scss文件,在分页任何位置,注意 style lang="scss" 需要设置 scss
App.vue // 应用配置，用来配置App全局样式以及监听
```

统一参照uni-app官方文档

### 初探uni-app

**添加分页**

1. 鼠标移动pages右键

2. 右键新建页面,注意检查pages.json文件会自动写入pages项,文件路径

   1. pages.json文件 style 为分页配置项,类似小程序 page.json
   2. 修改 pages.json 

   **知识点:  uni-app分页配置在style里面写,globalStyle写全部样式配置,在一个配置文件,这里是有区别原生小程序开发,具体参照uni-app文档进行配置** [pages.json](http://www.hcoder.net/tutorials/info/id/1339)

**配置项**

​	配置项 

```js
navigationBarBackgroundColor HexColor //#000000 导航栏背景颜色 
navigationBarTextStyle String white //导航栏标题颜色，仅支持 black/white 
navigationBarTitleText String  //导航栏标题文字内容 
navigationStyle String default //导航栏样式，仅支持 default/custom。 
backgroundColor HexColor //#ffffff 下拉窗口的背景色 微信小程序
```

​	全局配置 

```js
globalStyle: {
    //...全局配置
}
```

​	局部配置 参照  **pages.style**内容

```
> pages数组,决定谁排在前面
```

**修改pages导致文件查找失败,检查无误,重启编辑工具**

```js
pages:[
    {
    	path: "xxx/xxx/xxx",
        style: {
            //...局部配置
        }
    }
]
```

​	系统自带底部tabBar栏配置

**如果配置tabBar需要保证abBar第一个路由,配置在第一个pages的第一个**

```js
"tabBar": {
    "color": "#7A7E83",
    "selectedColor": "#3cc51f",
    "borderStyle": "black",
    "backgroundColor": "#ffffff",
    "list": [{
      "pagePath": "pages/component/index",
      "iconPath": "static/image/icon_component.png",
      "selectedIconPath": "static/image/icon_component_HL.png",
      "text": "组件"
    }, {
      "pagePath": "pages/API/index",
      "iconPath": "static/image/icon_API.png",
      "selectedIconPath": "static/image/icon_API_HL.png",
      "text": "接口"
    }]
  }
```