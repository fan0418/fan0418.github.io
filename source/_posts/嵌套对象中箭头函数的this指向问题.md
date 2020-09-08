
---
title: 嵌套对象中箭头函数的this指向问题
---
在某厂的笔试中遇到了一道代码分析题，关于this的指向问题
```javascript
let a = {
    name: 1
}
let b = {
    name: 2,
    say1() {
        console.log(this.name)
    },
    say2: () => console.log(this.name),
    say3() {
        let f = function () { console.log(this.name) }
        f()
    },
    say4() {
        let f = () => console.log(this.name)
        f()
    },
    say5() {
        let f = () => console.log(this.name)
        f.call(a)
    },
    say6(){
        let f=function(){console.log(this.name)}
        f.call(a)
    },
    c: {
        name: 3,
        say() {
            console.log(this.name)
        }
    }
}
const say=b.c.say
const say4=b.say4
say()
say4()
b.say1()
b.say2()
b.say3()
b.say4()
b.say5()
b.say6()
b.c.say()
```
答案当然很简单，主要是注意普通函数和箭头函数的this指向问题，我之前理解的普通函数this指向调用他的对象，箭头函数this指针指向上下文。
当然结果很简单，按照顺序分别是undefined，undefined，2，undefined，undefined，2，2，1，3，但是出于好奇，我在c中加了个方法。
```javascript
 c: {
        name: 3,
        say() {
            console.log(this.name)
        },
        say1:()=>console.log(this)
    }
```
如果执行b.c.say1(),结果会是什么呢？

按照我的理解，this指向上下文，那就是c的this，那c的this是b呀，应该输出2才对，结果代码在VSCODE里面一运行，输出了undefined，一时摸不着，因此又翻开阮一峰老师的ES6标准入门，书中说“函数体内的this对象就是定义时所在的对象，而不是使用时所在的对象。”还是有些模糊，又搜了一些帖子，得到了总结。

### 可以这么理解箭头函数的this指向，箭头函数的this与向上寻找的最近的非箭头函数的this相同。