



# 前端100问 [https://juejin.im/post/5d23e750f265da1b855c7bbe] 

## 写 React / Vue 项目时为什么要在列表组件中写 key，其作用是什么？
``` bash 
    在非常简单的的循环中
    let data = [1,2,3,4]
    <div>
        <div v-for='data'>{data}</div>
    </div>

    以上例子中 vue 会生成4个dom节点
    <div>1</div>
    <div>2</div>
    <div>3</div>
    <div>4</div>

    当data只进行内容变化的时候 比如说 1，2，3，4 => 变成 4，3，2，1的的时候 
    有key的时候 vue 会对dom位置进行调整 
        <div>4</div> id 4
        <div>3</div> id 3
        <div>2</div> id 2
        <div>1</div> id 1
    没有key的时候 vue只是会对dom节点的innerText 进行更改
        <div>4</div> id 1
        <div>3</div> id 2
        <div>2</div> id 3
        <div>1</div> id 4
    
    当data进行数据的增删改查的时候 data = [6，7，8，9]

    没有key得时候  Vue 节点位置不变，内容只是更新了
    有key的情况， 节点删除了 1 2 3 4   节点，新增了6，7，8，9节点 所以这样的话 

        从以上来看，不带有key，并且使用简单的模板，基于这个前提下，可以更有效的复用节点，diff速度来看 也是不带key更加快速的，因为带key在增删节点上有耗时。这就是vue文档所说的默认模式。
    但是这个并不是key作用，而是没有key的情况下可以对节点就地复用，提高性能。
        这种模式会带来一些隐藏的副作用，比如可能不会产生过渡效果，或者在某些节点有绑定数据（表单）状态，会出现状态错位。
    比如说循环渲染checkbox 选中莫个 并且删除了该选中的数据 就会发现该checkbox 并没有消失 而是 顺延到了下一位 这种就是不带key的弊端 就地复用的原则


    但是key的作用是什么呢 
        key是给每一个vnode的唯一id,可以依靠key,更准确, 更快的拿到oldVnode中对应的vnode节点。

        1 更准备
            如果有key就不是就地复用了 在vue sameNode函数中 newKey === oldKey 可以避免就地复用的情况。所以会更加准确。
        2 更快捷
            利用key的唯一性生成map对象来获取对应节点，比遍历方式更快。(这个观点，就是我最初的那个观点。从这个角度看，map会比遍历更快。)

    vue 是如何判断新旧节点
        vue 和react 都采用了diff算法，来对比新旧虚拟节点 进而更新节点
        那么如何比对呢 ???
        在交叉对比中当新旧节点 头尾交叉对比没有接口的时候 会判断是否有key  如果有key 会根据新节点的key去对比旧节点数组中的key 从而找到相应旧节点，如果没找到就认为是一个新增节点。
        而如果没有key，那么就会采用遍历查找的方式去找到对应的旧节点。一种一个map映射，另一种是遍历查找。相比而言。map映射的速度更快。

    
``` 

## ['1', '2', '3'].map(parseInt) what & why ?
## 什么是防抖和节流？有什么区别？如何实现？
## 介绍下 Set、Map、WeakSet 和 WeakMap 的区别？
## 介绍下深度优先遍历和广度优先遍历，如何实现？
## 请分别用深度优先思想和广度优先思想实现一个拷贝函数？
## ES5/ES6 的继承除了写法以外还有什么区别？
## setTimeout、Promise、Async/Await 的区别
## Async/Await 如何通过同步的方式实现异步
## 异步笔试题
    请写出下面代码的运行结果
    async function async1() {
        console.log('async1 start');
        await async2();
        console.log('async1 end');
    }
    async function async2() {
        console.log('async2');
    }
    console.log('script start');
    setTimeout(function() {
        console.log('setTimeout');
    }, 0)
    async1();
    new Promise(function(resolve) {
        console.log('promise1');
        resolve();
    }).then(function() {
        console.log('promise2');
    });
    console.log('script end');
## 三栏布局的实现及优缺点
布局方案 | 实现 | 优点 | 缺点 |
|  :----:  | :----:  |  :----:  | :----:  |
Float 布局 | 左中右三列 左右浮动 中间列设置 margin | 比较简单 兼容性较好 | 浮动脱离标准流 需要清楚浮动
Position布局 | 左中右无序 根据定位属性设置子元素位置 | 快捷 方便 | 元素以及后代元素脱离标准流 高度未知得情况下 可使用性比较差
Table 布局 | 左中右三列，父元素display: table;子元素display: table-cell;居中子元素不设宽度	 | 使用起来方便,兼容性也不存在问题	 | ① 无法设置栏边距；② 对seo不友好；③ 当其中一个单元格高度超出的时候，两侧的单元格也是会跟着一起变高的
栅格布局 | 左中右三列，父元素display: grid;利用网格实现	 | 最强大和最简单	 | 兼容性不好，IE10+上支持，而且也仅支持部分属性
Flex布局  | 左中右三列，父元素display: flex;两侧元素设宽；居中子元素flex: 1;	 | 比较完美	 | 存在IE上兼容性问题，只能支持到IE9以上


## 文字单行显示/三行显示
### 单行
    ```
        overflow: hidden;
        text-overflow:ellipsis;
        white-space: nowrap;
    ```
### 多行
    ```
        display: -webkit-box;
        -webkit-box-orient: vertical;
        -webkit-line-clamp: 3;
        overflow: hidden;
    ```
## 重绘和回流
### 重绘
    当Dom树中得一些元素 需要更新css 属性  而且这些属性只是影响元素得外观，风格 而不会引响布局得 称之为重绘 如 background-color color visibility ...
### 回流
    当Dom树中得一部分元素 因为莫写原因 需要重新绘制或者是重新构建 称之为回流（回流一定会引起重绘 但是重绘不一定会引起回流）
### 如何避免这些操作
+ 将多次改变样式属性得操作一次操作 尽量使用class 而不是style 一个一个设置
+ 将需要多次重排得元素 脱离标准流
+ 多次操作节点 ，尽量完成后添加到Dom中 比如说渲染html 尽量在所有数据渲染完生成html代码  一次性得添加到Dom中 而不是循环添加 每一行html
## 手写斐波那契数列及其优化
## 查看代码输出，什么是宏任务和微任务，都包括哪些？
    首先javascript 是一个单线程得脚本语言
    所以在执行一行代码得同事 必须不会存在同时执行另外一行代码 这就导致后面得代码一直在等待 页面处于假死状态。因为前面得代码没有执行完
    所以全部代码都是同步执行得话，是一个非常严重得问题，
    于是有了异步事件得概念
    在JavaScript中任务 分为宏任务和微任务
### 微任务
#|浏览器|Node|
|  :----:  | :----:  |  :----:  | :----:  |
| promise.then catch finally | √ | √
| process.nextTick | x | √
| MutationObserver | √ | x

### 宏任务
  #|浏览器|Node|
|  :----:  | :----:  |  :----:  | :----:  |
| setTimeout | √ | √
| setInterval | √ | √
| script 主体代码 |  √ | √
| I/O |  √ | √
| UI 交互事件 |  √ | √
### 执行顺序
1. 先执行主线程
2. 遇到宏任务 放到宏队列
3. 遇到微任务 放到微队列
4. 主线程执行完毕
5. 执行微任务队列 执行完毕
6. 先入先出 执行宏任务队列中得第一个任务 执行完毕 
7. 判断微队列是否有任务 如果有重复 5-6 如果没有 继续执行在一个宏任务队列  重复5 6 7
## 编写javascript深度克隆函数deepClone
## Vue路由的两种模式，介绍其原理和优缺点
### hash

### history
## 编写js事件绑定函数
## 手写去重函数
## HTML语义化
## CSS3新特性
## 闭包及其应用
## ES6新特性，追问了let、promise、class 等
## 简述webpack配置项
## 你所知道的排序算法，及其实现方式
### 冒泡排序
### 选择排序
### 插入排序
## Vue组件传参的几种方式
+ prop
+ emit
+ $children
+ $parent
+ vuex
+ eventBus
## 基于项目需求，如何需求调研，选择合适的框架及方案，详述过程
## 工作中遇到最困难的问题是什么
## 用户登录流程及权限判定，用户信息存储
## 路由跳转，页面如何刷新
## 盒模型类型及区别
## div垂直居中
## 怎样写一个可拖曳的div，怎样将他拖到其他节点内
## 现场画一个三行三列自适应布局
## BFC定义/作用/触发条件
## display的属性
+ none 隐藏元素 会造成元素得回流
+ block 块级元素 独占一行 宽高自己设置 
+ inline-block 行业块级元素 宽高自己设置  共同占一行 
+ inline 行业元素 宽高由内容决定 共同占一行
## 选择器
### 选择器有哪些
+ 通配符 * {}
+ 标签选择器 如 div {}
+ id 选择器
+ 类 选择器
+ 组合选择器 如 .test1 .test2 表示同时符合这两种得元素
+ 后代选择器
+ 子选择器 如 div>p{}
+ 伪类选择器 after befor
### 优先级
    !important => 行内样式=>ID选择器=>类选择器=>标签=> 通配符
## ES6语法:promise/箭头函数/class

## 闭包的用法和作用域
## 原型链，实现继承的方法
## 异步及其解决方案，宏任务和微任务及其流程
## 跨域
+ jsonp
+ 动态创建script 标签
+ proxy 代理 
+ nginx 反向代理
+ postMessage 跨域
+ window.name （name 值在不同的页面（甚至不同域名）加载后依旧存在，并且可以支持非常长的 name 值（2MB））
## 实现深拷贝
## 实现promise
## http的GET和POST区别/状态码
## webpack的打包原理、常用的loader和plugin，以及一些常用配置



