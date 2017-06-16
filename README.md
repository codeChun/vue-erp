# 教程简介
本教程要实现一个简单的后台管理系统，包含登陆、数据列表、数据查询、列表分页、添加数据、修改数据和删除数据等功能，教程会从 Vue 入门开始讲解，包含 es6、Sass、Webpack、Bootstrap、jQuery 等技术，再到后台管理系统的一些常规功能，用 Vue 如何去实现。也许会有人质疑 Vue 和 jQuery 的搭配，在我本人看来，jQuery 本身已很成熟，而且包含了很多非常好用的插件，比如表单验证，会比 Vue 的成本低很多。在本教程中会用纯 Vue 和 Vue + jQuery 两种方式去实现某些功能，可用于对比两种方案在开发成本和性能上到底有多大的影响。

# 后台管理系统的常规技术点
- 数据请求 ajax
  - 所有 ajax 请求都会有 loding 和遮罩层
  - 所有的 ajax 的 url 都应该设置相对应的前缀方便发布
- 数据列表 datagrid
  - 可配置是否需要分页
  - 可配置是否需要搜索功能
  - 可配置数据列表是否能修改、删除、添加和查看详情
  - 可配置水平滚动的固定列
  - 可配置点击列头是否需要排序
  - 可配置列表是否允许直接修改数据
  - 可配置列表是否允许复制行
  - 列表的数据在编辑时可自动生成对应的表单元素，比如编辑日期时会自动生成日期控件
  - 列表在编辑时的验证规则
  - 是否需要支持树形结构
- 表单 form
  - 信息分组显示
  - 表单编辑权限
  - 表单验证
  - 表单控件自动生成
  - 每行显示的元素数量
  - 跨列显示
- 数据完整性
- 数据安全性
- 数据查询性能
- 系统支持多语言切换
- 系统权限

# 技术点目录
- [认识 Vue](#vue)
- [认识数据驱动模式](#dd)
- [认识 MVVM 模式](#mvvm)
- [模版语法](https://github.com/dk-lan/vue-erp/VueBasic/模版语法)

# 认识 Vue{vue}
关于 Vue 的描述有不少，不外乎都会拿来与 Angular 和 React 对比，同样头顶 MVVM 双向数据驱动设计模式光环的 Angular 自然被对比的最多，但到目前为止，Angular 在热度上已明显不及 Vue，性能已成为最大的诟病。

在我看来，Vue 和 Angular 的对比有种早些年 Java 和 ASP.NET 的对比，对于开发者而言，ASP.NET 官方本身已实现好了大量的框架和功能，使用起来非常的方便快捷，同时也提供了无限的可扩展性，对比起 Java 而言，后者在本身框架和功能上都不及 ASP.NET，但同样都拥有无限的可扩展性，相比之下，本来 ASP.NET 很有一统天下的可能，但现实终归现实，ASP.NET 本身的框架和功能实现并没有换来多少称赞，反在性能和安全性方面被诟病。回看 Vue 和 Angular 的阵营，我也总有这么一种感觉。

所以，在这个开源的年代，我认为一个框架功能不需要有多么强大，再强大再完善的功能都抵不上“适合”两字，反而轻量级且有无限可扩展性会成为所有开发者的追求。

关于 Vue、React 和 Angular，其实都是在原生 JS 基础上，对面向对象不一样的实现方式而已，所以要想使用这三者中的任意一种，首先要有一定的 JS 基础和对面向对象有一定的认识。

在代码层面，Vue 只是一个构造函数，整个 Vue 的实现都在实例化这个构造函数开始。
``` javascript
  var vm = new Vue({
    //选项
  })
```

# 认识数据驱动模式{#dd}
开始接触前端编程的基本上都是先学习 DOM 节点操作，jQuery 在这一领域上一度成为了标准，所以在前端编程的领域中，jQuery 几乎是每个开发者的标配。随着前后端分离的成熟，产品或项目都趋于分布式部署，开发者已不满足于操作 DOM 节点， 设计模式便慢慢的被前端化。

数据驱动的设计模式在思维逻辑上与 DOM 节点操作完全不一样。

``` html
  <div id="div1"></div>
  <div id="app">
    <span>{{message}}</span>
  </div>
```
``` javascript
  //DOM 节点操作
  document.getElementById('div1').innerText = '节点被动改变'  

  //Vue 数据驱动： 当 message 发生改变的时候，span 会相应的发生改变，而不需要手动去改变 span。
  var vm = new Vue({
    el: '#app',
    data: {
      message: '我是通过映射显示的文本'
    }
  })

```

# 认识 MVVM 模式{#mvvm}
M：Model，称之为数据模型，在前端以对象的形式表现。
``` javascript
  var data = {message: '我就是一个数据模型'}
```
V：View，视图，也就是 HTML
``` html
  <div id="app">
    <span>我是视图</span>
  </div>
```
VM：ViewModel，就是连接数据和视图的桥梁，当 Model 发生改变的时候，ViewModel 便将数据映射到视图。

那么数据驱动模式和 MVVM 模式有什么关系，换句话说，MVVM 是数据驱动模式的一种实现，Vue 是 MVVM 的一种实现。