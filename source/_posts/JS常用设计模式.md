---
title: JS常用设计模式
date: 2020-04-11 15:41:54
tags:
    - Javascript
    - 面试
categories:
    - Notes
---
# 工厂模式
    工厂模式类似于现实生活中的工厂可以产生大量相似的商品，去做同样的事情，实现同样的效果;这时候需要使用工厂模式。
+ 优点：能解决多个相似的问题。
+ 缺点：不能知道对象识别的问题(对象的类型)。

```JS
function createPerson_Factory(name, age, job) {
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function () {
        console.log(this.name);
    };

    return o;
}

var person1 = createPerson_Factory("Ann",19,"Doctor");

```

# 单例模式
确保只有一个实例，并提供全局访问。在JS开发中，经常把用一个对象包裹,这样减少了全局变量的污染，比如 var a = {}。  
### 优点
1. 可以用来划分命名空间，减少全局变量的数量
2. 使用单体模式可以使代码组织的更为一致，使代码容易阅读和维护
3. 可以被实例化，且实例化一次

```JS
//实例对象总是在我们调用方法时才被创建，而不是在页面加载好的时候就创建。  
// 这样就不会每次点击按钮，都会创建一个 div 了
var createLayer2 = function () {
var div;
return function () {
   if (!div) {
     document.createElement('div');
     div.innerHTML = '我是内容';
     div.style.display = 'none';
     document.body.appendChild(div);
   }
   return div;
   }
}

document.getElementById('#btn').onclick = function () {
   var layer2 = createLayer2();
   layer2.style.display = 'block';
}
```

```JS
// 实现单体模式弹窗
var createWindow = (function(){
    var div;
    return function(){
        if(!div) {
            div = document.createElement("div");
            div.innerHTML = "我是弹窗内容";
            div.style.display = 'none';
            document.body.appendChild(div);
        }
        return div;
    }
})();
document.getElementById("Id").onclick = function(){
    // 点击后先创建一个div元素
    var win = createWindow();
    win.style.display = "block";
}
```


# 模块模式
    如果我们必须创建一个对象并以某些数据进行初始化，同时还要公开一些能够访问这些私有数据的方法，那么我们这个时候就可以使用模块模式了。
    (思路:为单体模式添加私有变量和私有方法能够减少全局变量的使用)
```JS
var singleMode = (function(){
    // 创建私有变量
    var privateNum = 112;
    // 创建私有函数
    function privateFunc(){
        // 实现自己的业务逻辑代码
    }
    // 返回一个对象包含公有方法和属性
    return {
        publicMethod1: publicMethod1,
        publicMethod2: publicMethod1
    };
})();
```


# 代理模式
### 优点
1. 代理对象可以代替本体被实例化，并使其可以被远程访问
2. 它还可以把本体实例化推迟到真正需要的时候；对于实例化比较费时的本体对象，或者因为尺寸比较大以至于不用时不适于保存在内存中的本体，我们可以**推迟实例化**该对象

```JS
// 先申明一个奶茶妹对象
var TeaAndMilkGirl = function(name) {
    this.name = name;
};
// 这是京东ceo先生
var Ceo = function(girl) {
    this.girl = girl;
    // 送结婚礼物 给奶茶妹
    this.sendMarriageRing = function(ring) {
        console.log("Hi " + this.girl.name + ", ceo送你一个礼物：" + ring);
    }
};
// 京东ceo的经纪人是代理，来代替送
var ProxyObj = function(girl){
    this.girl = girl;
    // 经纪人代理送礼物给奶茶妹
    this.sendGift = function(gift) {
        // 代理模式负责本体对象实例化
        (new Ceo(this.girl)).sendMarriageRing(gift);
    }
};
// 初始化
var proxy = new ProxyObj(new TeaAndMilkGirl("奶茶妹"));
proxy.sendGift("结婚戒"); // Hi 奶茶妹, ceo送你一个礼物：结婚戒
```
### 使用虚拟代理实现图片的预加载


# 策略模式
便于修改
```JS
// 普通模式
var awardS = function (salary) {
    return salary * 4
};

var awardA = function (salary) {
    return salary * 3
};

var awardB = function (salary) {
    return salary * 2
};

var calculateBonus = function (level, salary) {
   if (level === 'S') {
    return awardS(salary);
   }
   if (level === 'A') {
    return awardA(salary);
   }
   if (level === 'B') {
    return awardB(salary);
   }
};

calculateBonus('A', 10000);
```
```JS
  var strategies = {
    'S': function (salary) {
      return salary * 4;
    },
    'A': function (salary) {
      return salary * 3;
    },
    'B': function (salary) {
      return salary * 2;
    }
  }

  var calculateBonus = function (level, salary) {
    return strategies[level](salary);
  }

  calculateBonus('A', 10000);
```


# 职责链模式
### 优点  
消除请求的发送者与接收者之间的耦合。  
### 流程 
1. 发送者知道链中的第一个接收者，它向这个接收者发送该请求。
2. 每一个接收者都对请求进行分析，然后要么处理它，要么它往下传递。
3. 每一个接收者知道其他的对象只有一个，即它在链中的下家(successor)。
4. 如果没有任何接收者处理请求，那么请求会从链中离开。
```JS
// 我们一般写代码如下处理操作
var order =  function(orderType,isPay,count) {
    if(orderType == 1) {  // 用户充值500元到支付宝去
        if(isPay == true) { // 如果充值成功的话，100%中奖
            console.log("亲爱的用户，您中奖了100元红包了");
        }else {
            // 充值失败，就当作普通用户来处理中奖信息
            if(count > 0) {
                console.log("亲爱的用户，您已抽到10元优惠卷");
            }else {
                console.log("亲爱的用户，请再接再厉哦");
            }
        }
    }else if(orderType == 2) {  // 用户充值200元到支付宝去
        if(isPay == true) {     // 如果充值成功的话，100%中奖
            console.log("亲爱的用户，您中奖了20元红包了");
        }else {
            // 充值失败，就当作普通用户来处理中奖信息
            if(count > 0) {
                console.log("亲爱的用户，您已抽到10元优惠卷");
            }else {
                console.log("亲爱的用户，请再接再厉哦");
            }
        }
    }else if(orderType == 3) {
        // 普通用户来处理中奖信息
        if(count > 0) {
            console.log("亲爱的用户，您已抽到10元优惠卷");
        }else {
            console.log("亲爱的用户，请再接再厉哦");
        }
    }
};
```
```JS
//职责链
function order500(orderType,isPay,count){
    if(orderType == 1 && isPay == true)    {
        console.log("亲爱的用户，您中奖了100元红包了");
    }else {
        //我不知道下一个节点是谁,反正把请求往后面传递
        return "nextSuccessor";
    }
};
function order200(orderType,isPay,count) {
    if(orderType == 2 && isPay == true) {
        console.log("亲爱的用户，您中奖了20元红包了");
    }else {
        //我不知道下一个节点是谁,反正把请求往后面传递
        return "nextSuccessor";
    }
};
function orderNormal(orderType,isPay,count){
    // 普通用户来处理中奖信息
    if(count > 0) {
        console.log("亲爱的用户，您已抽到10元优惠卷");
    }else {
        console.log("亲爱的用户，请再接再厉哦");
    }
}
// 下面需要编写职责链模式的封装构造函数方法
var Chain = function(fn){
    this.fn = fn;
    this.successor = null;
};
Chain.prototype.setNextSuccessor = function(successor){
    return this.successor = successor;
}
// 把请求往下传递
Chain.prototype.passRequest = function(){
    var ret = this.fn.apply(this,arguments);
    if(ret === 'nextSuccessor') {
        return this.successor && this.successor.passRequest.apply(this.successor,arguments);
    }
    return ret;
}
//现在我们把3个函数分别包装成职责链节点：
var chainOrder500 = new Chain(order500);
var chainOrder200 = new Chain(order200);
var chainOrderNormal = new Chain(orderNormal);

// 然后指定节点在职责链中的顺序
chainOrder500.setNextSuccessor(chainOrder200);
chainOrder200.setNextSuccessor(chainOrderNormal);

//最后把请求传递给第一个节点：
chainOrder500.passRequest(1,true,500);  // 亲爱的用户，您中奖了100元红包了
chainOrder500.passRequest(2,true,500);  // 亲爱的用户，您中奖了20元红包了
chainOrder500.passRequest(3,true,500);  // 亲爱的用户，您已抽到10元优惠卷 
chainOrder500.passRequest(1,false,0);   // 亲爱的用户，请再接再厉哦
```
### 异步职责链


# 命令模式


# 参考
[Javascript十大常用设计模式](https://blog.csdn.net/zm06201118/article/details/102638474?depth_1-utm_source=distribute.pc_relevant.none-task-blog-OPENSEARCH-4&utm_source=distribute.pc_relevant.none-task-blog-OPENSEARCH-4)
[Javascript设计模式详解](https://www.cnblogs.com/tugenhua0707/p/5198407.html)