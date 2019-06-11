#这是我修改过后的版本
# Vue-Element-Admin
[![LICENSE](https://img.shields.io/badge/license-Anti%20996-blue.svg)](https://github.com/996icu/996.ICU/blob/master/LICENSE)
![Prs](https://img.shields.io/badge/Prs-welcome-brightgreen.svg)
![author](https://img.shields.io/badge/author-jasonbai-orange.svg)
[![996.icu](https://img.shields.io/badge/link-996.icu-red.svg)](https://996.icu)

## Build Setup

``` bash
# install dependencies
yarn install

# serve with hot reload at localhost:1024
yarn run dev

# build for production with minification
yarn run build

# visualize output files
yarn run analysis

# preview output files
yarn add http-server -g
cd dist
http-server
```

## Tech Stack
* Vue @ 2.5.9
* Vue-Router
* Vue-cli
* Vue-i18n
* Vuex
* ES6
* Axios
* jsonp
* eCharts @ 4.2.1
* Element-UI @ 2.8.2
* Sass (怀念Less)
* Animate.CSS
* Typed.js
* Font-Awesome
* MockJS
* Yarn
* babel-polyfill 兼容IE垫片

## Others Maybe Need
* intro.js 引导页
* vue-qr  二维码生成模块
* vuedraggable  拖拽功能
* vue-particles  粒子星空效果
* xlsx  读取excel表
* js-cookie 读写cookie模块
* dayjs 日期处理模块
* rythm.js 音频节奏可视化模块
* vuescroll.js 一个滚动条插件
* crypto-js 加密算法，登录时使用
* clipboard.js 复制粘贴
* vue-baidu-map 封装的百度地图插件

## Notes
1. 登录页已兼容IE浏览器
2. 由于引入了rythm.js，导致其它页面不兼容IE，使用时，删除Siderbar.vue里面关于rythm.js的代码就可以了
3. 配置webpack.base.conf.js中的alias的vue字段，可以解决IE浏览器中的报错问题。（这块儿不用动了，已调整好）
4. 背景音乐的歌名是：Romeo's Tune
5. node-sass经常安装失败，需要多次尝试，如果实在不行，最好翻墙后再次 yarn install。
6. 如果要覆盖饿了么组件的样式变量，就编辑assets/scss/element-variables.scss文件。
7. 因为谷歌浏览器升级到71版本，导致screenfull.js源码98行有问题，已经将改动后的文件复制到了assets/libs文件夹下。
8. 彻底重写了图表组件，提高了组件的扩展性，图表配置需要在使用组件的父组件中设置。

## Useful Links
* [饿了么组件](http://element.eleme.io/#/zh-CN/component/installation)
* [Vue官网](https://cn.vuejs.org/v2/guide/)
* [axios官网](https://github.com/mzabriskie/axios)
* [Vue-loader使用指南](http://vue-loader.vuejs.org/en/)
* [Vue-cli使用指南](http://vuejs-templates.github.io/webpack/)
* [一位大神的vue项目搭建干货总结](https://linjinze999.github.io/vue-llplatform/)
* [Vue 组件间通信六种方式（完整版）](https://juejin.im/post/5cde0b43f265da03867e78d3)
* [7个有用的Vue开发技巧](https://juejin.im/post/5ce3b519f265da1bb31c0d5f)
* [手摸手，带你用vue撸后台 系列五](https://juejin.im/post/5c92ff94f265da6128275a85)

## Folder Instructions
| Name | Remark |
| - | - |
| .vscode | VScode的sftp插件配置文件，用于将编译后文件传输到服务器上 |
| dist | 打包后的文件所在目录 |

## Login Logic
1. 请求登录接口，获取到token、用户ID等信息，保存到localStorage或Cookie里
2. 请求左侧菜单信息。（如果菜单是固定的，这步忽略）
3. 登录成功后，导航到首页
4. 在main.js里设置全局请求拦截，在所有请求的header里加上token信息，证明当前已登录
5. 在main.js里设置全局接收拦截，如果服务器端的token已过期，返回401，就清空localStorage并重定向到登录页面
6. 在路由配置表里，设置全局路由拦截，如果token存在并且要去往登录页面，就不让去，并且重定向到首页

## Logout Logic
1. 点击退出按钮，清空localStorage，并导航到登录页面

## First Open Logic
1. 配置路由表，'/'重定向到首页
2. 如果没有登录（本地没有token），首页的请求就会是401，这时候，就会导航到登录页
3. 如果已经登录过，本地有token，就待在首页，不用再登录一遍了

## Change Log
2019-06-04
* 优化目录结构
* 美化404页面
* 美化登录页
* 添加空白页面
* 实现动态路由添加

2019-05-28
* 单独引入echarts
* 优化全局过滤器的写法
* 优化全局bus注册方法

2019-05-05
* 升级饿了么组件库到最新版
* 彻底重写了图表组件

2018-08-06
* 修复退出后音乐不停止bug
* 优化页面Logo跳舞效果
* 修改路由配置，首次进入到登录页面

2018-07-15
* 更新Readme
* 优化登录登出逻辑
* 删除多余模块
* 兼容IE浏览器

2018-07-10
* 优化打包生成目录
* 实现打包文件可视化
* 修复登录页报错bug
* 在入口处引入mock.js
* 删除左侧导航动画效果
* 优化了package.json中的script脚本
* 优化各个组件的文件名
* 饿了么组件库升级到了最新版本@2.4.3

## License
Anti-996
