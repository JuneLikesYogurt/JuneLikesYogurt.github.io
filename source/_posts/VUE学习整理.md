---
title: VUE学习整理
date: 2020-09-17 10:46:27
tags:
    - VUE
categories:
    - Notes
---

## VUE基础

### 计算属性 & 方法
计算属性是基于它们的响应式依赖进行缓存
```HTML
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```
```JS
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
```


### 计算属性 & 侦听属性
**computed**
**watch**，大多数情况下计算属性更合适。  
有时也要用自定义的侦听器。（当需要在数据变化时执行异步操作or开销较大的操作时）
https://segmentfault.com/a/1190000012948175?utm_source=tag-newest
https://www.cnblogs.com/jiajialove/p/11327945.html


### v-show & v-show
**v-show** 的元素始终会被渲染并保留在DOM中，它只是简单地切换CSS的display
**v-if** 是真正的条件渲染，会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁重建
一般来说，v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。

### v-for
  **v-for**中可以访问所有的父作用域的property


### v-on
  **v-on**可以监听DOM事件（alert、event.preventDefault、鼠标键盘按键），并在触发时运行一些JS代码


### 组件
#### prop
<!-- 在组件上注册的一些自定义 attribute   -->
prop的作用是父组件向子组件单向传递数据，这个过程是单向的。
传递的属性可以是静态的，可以是动态的，可以是数字，可以是字符串，可以是数组，还可以是对象，甚至可以在传递数据的时候写一个校验函数进行校验。
#### 在组件上使用v-model
```HTML
<input v-model="searchText">
```
等价于
```HTML
<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
>
```

### v-model和v-bind区别
**v-bind**,`v-bind:class`可简写为`:class`，单向绑定，一般对HTML中的属性、css的样式、对象、数组、number类型、bool类型绑定
**v-model**,双向绑定，主要用在表单中


### 插槽
插槽就是子组件中的提供给父组件使用的一个占位符，用`<slot></slot>` 表示，父组件可以在这个占位符中填充任何模板代码，如 HTML、组件等，填充的内容会替换子组件的`<slot></slot>`标签。
插槽 prop 允许我们将插槽转换为可复用的模板，这些模板可以基于输入的 prop 渲染出不同的内容。`这在设计封装数据逻辑同时允许父级组件自定义部分布局的可复用组件时是最有用的。`
[插槽的理解和使用](https://www.cnblogs.com/mandy-dyf/p/11528505.html)


### ref 访问子组件实例或子元素
尽管存在 prop 和事件，有的时候你仍可能需要在 JavaScript 里直接访问一个子组件。为了达到这个目的，你可以通过 **ref** 这个 attribute 为子组件赋予一个 ID 引用。例如：
```javascript
<base-input ref="usernameInput"></base-input>
```  
`this.$refs.usernameInput`
\$refs 只会在组件渲染完成之后生效，并且它们不是响应式的。这仅作为一个用于直接操作子组件的“逃生舱”——你应该避免在模板或计算属性中访问 $refs。


---
## Vue Router
### 动态路由匹配
```javascript
const User = {
  template: '<div>User</div>'
}

const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id', component: User }
  ]
})
```
当使用路由参数时，例如从 /user/foo 导航到 /user/bar，原来的组件实例会被**复用**。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。不过，这**也意味着组件的生命周期钩子不会再被调用。**
**复用组件时，想对路由参数的变化作出响应的话，你可以简单地 watch (监测变化) $route 对象。**

### 嵌套路由
要在嵌套的出口中渲染组件，需要在 VueRouter 的参数中使用 children 配置：  
```javascript
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User,
      children: [
        {
          // 当 /user/:id/profile 匹配成功，
          // UserProfile 会被渲染在 User 的 <router-view> 中
          path: 'profile',
          component: UserProfile
        },
        {
          // 当 /user/:id/posts 匹配成功
          // UserPosts 会被渲染在 User 的 <router-view> 中
          path: 'posts',
          component: UserPosts
        }
      ]
    }
  ]
})
```


### 导航守卫

#### 全局前置守卫
`router.beforeEach`
```javascript
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // ...
})
```
+ `to: Route`,即将要进入的目标 路由对象
+ `from: Route`,当前导航正要离开的路由
+ `next: Function`,一定要调用该方法来 resolve 这个钩子。执行效果依赖 next 方法的调用参数。
  + `next()`: 进行管道中的下一个钩子。
  + `next(false)`: 中断当前的导航。
  + `next('/')` or `next({ path: '/' })`: 跳转到一个不同的地址
  + `next(error)`: (2.4.0+) 如果传入 next 的参数是一个 Error 实例，则导航会被终止且该错误会被传递给 router.onError() 注册过的回调。

### 完整导航解析流程
1. 导航被触发。
2. 在失活的组件里调用 beforeRouteLeave 守卫。
3. 调用全局的 **beforeEach** 守卫。
4. 在重用的组件里调用 beforeRouteUpdate 守卫 (2.2+)。
5. 在路由配置里调用 beforeEnter。
6. 解析异步路由组件。
6. 在被激活的组件里调用 beforeRouteEnter。
7. 调用全局的 beforeResolve 守卫 (2.5+)。
8. 导航被确认。
7. 调用全局的 **afterEach** 钩子。
8. 触发 DOM 更新。
9. 调用 beforeRouteEnter 守卫中传给 next 的回调函数，创建好的组件实例会作为回调函数的参数传入。


---
## 状态管理Vuex
采用**集中式存储**管理应用的所有组件的状态
并以相应的规则保证状态以一种可预测的方式发生变化

[Vuex，从入门到入门](https://zhuanlan.zhihu.com/p/24357762)
（参考的大佬写得太好了，就直接点链接看吧）
