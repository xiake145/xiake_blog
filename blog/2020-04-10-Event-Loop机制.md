---
id: event loop
title: Event Loop
author: xiake
author_url: https://github.com/xiake145
author_image_url: https://avatars1.githubusercontent.com/u/50768947
tags: [js小技巧]
---
<!--truncate-->
> js是一门单线程语言 它的多线程是通过Event Loop事件循环机制来实现的。  
大体由三个部分组成
>> 调用栈`call stack` 消息队列`Message Queue` 微任务队列`Microtask Queue`  

Event Looop 开始时会从全局代码开始，一行一行执行，遇到函数调用 会把它压入函数调用栈中
被压入的函数叫做帧(Frame)`fn1`,当函数返回后会从调用栈中弹出、输出。
```js
    function fn1(){
        console.log(1)
    }
    function fn2(){
        console.log(2)
        fn1()
        console.log(3)
    }
    fn2()
    //2,1,3
```
上面示例会将fn2执行，将其压入栈中，并执行里面的代码，遇到clg2，将后clg2压入栈中，输出，再将fn1压入栈中，输出1，将fn1，弹出，然后执行clg3，fn2执行完毕将fn2弹出
```js
    function fn1(){
        console.log(1)
    }
    function fn2(){
        setTimeout(()=>console.log(2),0)
        fn1()
        console.log(3)
    }
    fn2()
    //2,1,3
```
加上setTimeout(setinterval,事件回调)后，里面的回调函数会入队到消息队列中，称为消息。当setTimeout被压入栈中，里面的回调函数会入队到消息队列，消息会在调用栈清空的时候(最小延迟时间)执行，将消息队列里的消息会被压入到调用栈中执行。
> 使用promise async await创建的异步操作会加入到微任务队列中，他会在调用栈清空的时候会立即执行，并且再处理期间新加入的微任务也会一同执行。最后再执行消息队列
