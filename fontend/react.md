# react

### 基本原理

react中通过原始js对象的方式来管理dom，概念是element，每个element都是一个原始js对象，有两个一级属性

- 第一个是type，数值可以是其他element的实例或者是原始dom标签string(div,span)
- 第二个是props，管理当前element的属性，重要的属性有children，classname

通过element的一层层叠加，创建出整个virtual dom tree结构，来映射实际的dom

### 创建React元素的三种方式
- 纯function方式，方法参数是props,返回一个object,一级属性有type和props
- 使用react.createClass工厂方法， 在该方法填入一个object参数，这个object有一个render方法，返回一个和第一种方法一样的object
- （最常用）用es6中的class语法糖创建，继承React.Component，该类又一个render方法

## react词汇概念解释

#### sp单页应用
单页应用只有一个HTML页面，和页面的所有交互或者页面跳转也不会走到服务端
