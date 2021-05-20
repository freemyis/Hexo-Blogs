---
title: webpack项目使用prerender-spa-plugin实现vue单页面打包多页面SEO优化
date: 2021-05-20 13:36:54
tags: SEO vue webpack
---
> ## vue项目引入prerender-spa-plugin
### seo为啥对vue单页面不友好?
```
1. 爬虫在爬取的过程中，不会去执行js，所以隐藏在js中的跳转也不会获取到
2. vue通过js控制路由然后渲染出对应的页面，而搜索引擎蜘蛛是不会去执行页面的js的，导致搜索引擎蜘蛛只能
收录index.html一个页面，在百度中就搜索不到相关的子页面的内容。
3. 我们加载页面的时候,浏览器的渲染包含:html的解析、dom树的构建、cssom构建、javascript解析、布局、绘制,
当解析到javascript的时候才回去触发vue的渲染,然后元素挂载到id为app的div上,这个时候我们才能看到我们页面
的内容,所以即使vue渲染机制很快我们仍然能够看到一段时间的白屏情况,用户体验不好
```
### 优化过程记录
#### 基础优化 
```
1. 对index.html添加合理的title、description、keywords
2. 语义化的html,给图片alt添加合适的内容
3. 向各大网站收录自己的网站
```
#### 安装prerender-spa-plugin插件
```
cnpm install prerender-spa-plugin --save
//在vue.config.js中添加相应配置
const path = require("path");
const PrerenderSPAPlugin = require("prerender-spa-plugin");
const Renderer = require("@prerenderer/renderer-jsdom");
module.exports = {   
  configureWebpack: () => {
    if (process.env.NODE_ENV !== "production") return;
    return {
      plugins: [
        new PrerenderSPAPlugin({
          // 生成文件的路径，也可以与webpakc打包的一致。
          // 下面这句话非常重要！！！
          // 这个目录只能有一级，如果目录层次大于一级，在生成的时候不会有任何错误
          提示，在预渲染的时候只会卡着不动。
          staticDir: path.join(__dirname, "dist"),
          indexPath: path.join(__dirname, 'dist', 'index.html'),
          // 对应自己的路由文件，比如a有参数，就需要写成 /a/param1。
          routes: ["/", "/gisPlatForm", "/bimPlatForm", "/iotPlatForm"],
          // 这个很重要，如果没有配置这段，也不会进行预编译
          renderer: new Renderer({
            inject: {
              foo: "bar"
            },
            headless: false,
            // 在 main.js 中document.dispatchEvent(newEvent('render-event'))，
            //两者的事件名称要对应上。
            renderAfterDocumentEvent: "render-event"
          })
        })
      ]
    };
  }
};
//在main.js中添加相关引入
new Vue({
  router,
  render: h => h(App),
  mounted() {
    document.dispatchEvent(new Event("render-event"));
  }
}).$mount("#app");
///最重要一点
router中必须是history模式。
```
#### 继续SEO优化，为每个页面设置keywords等配置
```
//安装vue-meta-info
npm install vue-meta-info --save
//在main.js中引用
import MetaInfo from "vue-meta-info";
Vue.use(MetaInfo);
//之后在需要的vue页面中添加以下内容
export default {
  metaInfo: {
    title: "message",
    meta: [
      {
        name: "description",
        content: "首页"
      },
      {
        name: "keywords",
        content: "官网首页,我最帅,哈哈哈"
      }
    ]
  }
}
```
##### 奖励自己一个妹子，哈哈哈
![妹子](https://tse1-mm.cn.bing.net/th?id=OIP.rr8-sQN7vY5rMG3dk3khCgHaNK&w=107&h=160&c=8&rs=1&qlt=90&pid=3.1&rm=2)
---
>#### 使用插件过程中遇到的坑，填坑之路漫漫啊，其修远兮。
```
1. vue.config.js提示找不到Renderer方法
去GitHub上找到了prerenderer的项目，进行了npm下载--- npm i @prerenderer/prerenderer
//搞定问题，怎是一个爽字了得，哈哈哈
2. 打包完成没问题，生成多个.html页面不同的文件夹中，发布到服务器发现无法跳转
蒙逼中...内心是崩溃的，继续看prerender-spa-plugin插件的readme找找思路，诶嘿
发现了一个配置，indexPath: path.join(__dirname, 'dist', 'index.html'),有点
意思了
// Optional - The location of index.html 作者注释是这样说的，专门设置了主页面的位置
然后我又去找了找网站的问题发现了一些特别的东西
```
[以下引用自知乎自由的囚徒](https://zhuanlan.zhihu.com/p/272037693)
```
prerender-spa-plugin的一些坑
路由生成对应的页面(.html)时某些数据没有被渲染出来
在根组件上添加data-server-rendered ='true'
<div id="app" data-server-rendered="true"></>
直接访问某个为.html文件的后缀时只能渲染出静态页面，不会渲染到对应的路由。
```
```
综合以上的问题，我给index.html页面添加了data-server-rendered="true"，
成功解决，果然是站在巨人的肩膀上的感觉。
```
---
> ## 总结
```
1. 通过这次SEO优化熟悉了prerender-spa-plugin的机制和相关用法，踩了一鞋坑，不过收获非常大
2. 自己的知识面还是太窄，还需要继续学习，并应用到实践中才能够真正理解这些知识的原理
```
![飞鸟妹子结尾](https://tse1-mm.cn.bing.net/th/id/OIP.oqBtbcpqYCUKwg3DPpMEBwHaJQ?w=200&h=250&c=7&o=5&pid=1.7)