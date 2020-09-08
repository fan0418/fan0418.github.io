---
title: 原生js实现bind以及相关思考
---
在js的使用过程中，this指针的指向一直是一个影响我们代码编写的重要因素。那么为了改变this指针的指向，我们经常会用到call.apply.bind方法，可是仅仅知道这些方法的用途是远远不够的，因此还是需要自己动手去实现一下。

首先实现一个call。
```javascript
Function.prototype.mycall=function(context)
{
    if(typeof this !=='function')
    {
        throw new TypeError('Error')
    }
    context=context||window
    context.fn=this
    let arg=[...arguments].slice(1)
    let result=context.fn(arg)
    delete context.fn
    return result
}
```
类似实现一个apply,参数处理会不同，apply的第二个可选参数是一个数组
```javascript
Function.prototype.myapply=function(context)
{
    if(typeof this!=='function')
    {
        throw new TypeError('ERROR')
    }
    context=context|window
    context.fn=this
    let result
    if(arguments[1])
    {
       result=context.fn(...arguments[1])   
    }else{
        result=context.fn()
    }
    delete context.fn
    return result
}
```
再接着实现一个bind
```javascript
Function.prototype.mybind=function(context)
{
    if(typeof this!=='function')
    {
        throw new TypeError('ERROR')
    }
    context=context||window
    let arg=[...arguments].slice(1)
    let _this=this
    let F=function(){};
    F.prototype=this.prototype
    let res=function(){
        let innerArgs=[...arguments].slice(0)
        let finalArgs=arg.concat(innerArgs)
        return _this.apply(this instanceof F?this:context,finalArgs)
    }
    res.prototype=new F()
    return res
}
```
这里要注意，如果使用了new来构造绑定函数，则需要忽略此时bind中的第一个参数，new的实例的this指向new的那个对象。