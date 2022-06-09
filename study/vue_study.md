## 条件与循环
v-if v-for

## 处理用户收入
v-on:xxxx  v-model

> VUE里头的主要元素el，data，methods


## 组件化应用构建
```
// 定义名为 todo-item 的新组件
Vue.component('todo-item', {
  template: '<li>这是个待办项</li>'
})

var app = new Vue(...)
```

## VUE实例
### 实例生命周期钩子
create, updated, mounted, destroyed

## 模板语法
### 插值
{{}} v-once

### 原始html
v-html

### javascript表达式
{{}} 支持JavaScript表达式

## 指令
### 参数

### 动态参数

### 修饰符

## 计算属性和侦听器
### 计算属性
computed

### 侦听器
watch

## Class与style的绑定
v-bind:class
### 对象语法
没看懂

### 数组语法

### 用在组件上

## 绑定内联样式

v-bind:style


## 条件渲染
v-if v-else v-else-if

### 用key管理可以复用的元素
key属性的使用


## 列表渲染
v-for

### 显示过滤排序后的内容
使用计算属性 computed


## 时间处理