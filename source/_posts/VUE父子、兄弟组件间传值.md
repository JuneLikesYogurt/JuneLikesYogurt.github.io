---
title: VUE父子、兄弟组件间传值
date: 2020-02-28 14:41:09
tags:
    - VUE
categories:
    - Notes
---
[什么时候需要父子兄弟传值](https://segmentfault.com/q/1010000023655790)

<!-- more-->
---

## 父传子
父子传值一般在子组件上绑定自定义的属性，然后子组件在实例中通过props接收该属性  
**app.vue**
```VUE
<brother :name="name" />
```
**brothe.vue**
```JS
export default {
    name:'brother',
    props:['name'],
    data () {
        return {
           
        };
    }
}
```

## 子传父
一般使用emit反向传值，通过注册自定义事件，在事件函数中接收值  
**brother.vue**
```VUE
<template>
    <div>
        <div @click="aaa">click</div>
    </div>
</template>
```
```JS
export default {
    name:'brother',
    data () {
        return {
           
        };
    },
    aaa(){
        //注册自定义事件，并传递参数
        this.$emit('custom-event','你好，我是子组件传过来的值')
    }
}
```
**app.vue**
```vue
<template>
    <div>
        //在对应的子组件上，绑定自定义事件
        //事件被触发后，会调用bbb()函数
        <brother @custom-event="bbb" />
    </div>
</template>
```
```JS
export default {
    name:'app',
    data () {
        return {
           
        };
    },
    bbb(params){
        //自定义事件的回调函数中  会接受一个参数  来自于注册事件时传递的参数
        console.log(params)  //输出结果：你好，我是子组件传过来的值
    }
}
```

## 非父子组件间
建立一个公共的js文件，作为中间仓库来传值
**公共bus.js**
```JS
import Vue from 'vue'
export default new Vue()
```
**组件A**
```JS
<template>
  <div>
    // A组件:
    <span>{{elementValue}}</span>
    <input type="button" value="点击触发" @click="elementByValue">
  </div>
</template>

<script>
  // 引入公共的bug，来做为中间传达的工具
  import Bus from './bus.js'
  export default {
    data () {
      return {
        elementValue: 4
      }
    },
    methods: {
      elementByValue: function () {
        Bus.$emit('val', this.elementValue)
      }
    }
  }
</script>
```
**组件B**
```JS
    <template>
      <div>
        B组件:
        <input type="button" value="点击触发" @click="getData">
        <span>{{name}}</span>
      </div>
    </template>

    <script>
      import Bus from './bus.js'
      export default {
        data () {
          return {
            name: 0
          }
        },
        mounted: function () {
          var vm = this
          // 用$on事件来接收参数
          Bus.$on('val', (data) => {
            console.log(data)
            vm.name = data
          })
        },
        methods: {
          getData: function () {
            this.name++
          }
        }
      }
    </script>

```

# 参考
[vue组件间的传值（父子、子父、兄弟、后代、非兄弟）](https://blog.csdn.net/qq_43161149/article/details/89191564)  
[Vue传值（3种常用传值方法）](https://blog.csdn.net/jgs525/article/details/90453726?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)