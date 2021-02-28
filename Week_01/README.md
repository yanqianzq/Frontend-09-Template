学习笔记

典型案例：
红绿灯问题：一个路口的红绿灯，会按照绿灯亮10秒，黄灯亮2秒，红灯亮5秒的顺序无限循环，请编写JS代码来控制这个红绿灯。

JavaScript的异步机制
1、callback：利用函数回调的能力，去做异步机制；
2、Promise： 来自于Promise/A+这样一个更大的跨语言的设计，是很多语言都会用到的一种异步处理机制；
3、async/await：是基于Promise上面的一种语法上面的支持和封装，它在运行时实际也是靠Promise去管理异步的一种机制。

Promise和callback的区别是：
没有嵌套关系，它是一个链式表达式的形式，比回调的方式更友好一些.
promise后面其实蕴含了一些编程思想，我们可以通过Promise.all()来让它同时等待多个Promise结束之后再去执行;Promise.race()让几个Promise并行执行，只要有一个返回了就认为执行完毕；



async function goAsync() {
    while(true) {
        green();
        await(sleep(10000));
        yellow();
        await(sleep(2000));
        red();
        await(sleep(5000));
    }
}


async/await：通过给函数的前面加上async关键字，这个函数里面就可以使用await，从而可以等待一个Promise的结束，这样就可以使用大家比较熟悉的分支、循环和顺序三种结构去描述异步的关系，本质是一种纯粹从语法层面的一种改进，这个非异步同步代码的async/await能够描述同步代码的分支顺序循环。


采用Promise 和 async/await的好处是，随时可以把sleep()替换成某种事件；async/await的好处是可以像同步代码一样使用异步代码。



附：
generator模拟async/await函数实现异步，generator模拟async函数要早一些，早年会采用yield取模拟async/await。会再函数后面加个*，表示这是个generator函数，把await换成yield，但是需要一个逻辑去run它。

// 方法四：generator
function* goGenerator() {
    while(true) { //无限循环
        green();
        yield sleep(10000);
        yellow();
        yield sleep(2000);
        red();
        yield sleep(5000);
    }
}
goGenerator();

// generator函数会返回iterator迭代器
 //每次将iterator取出来，如果是done ,就结束；如果返回的是一个Promise就再执行run(iterator)操作
function run(iterator) {
    let { value, done } = iterator.next();
    if(done) return;
    if(value instanceof Promise) {
        value.then(() => {
            run(iterator);
        })
    }
}

//模仿了著名的框架co，通过co都把这个yield自动当作await去执行了。
function co(generator) {
    return function() {
        return run(generator());
    }
}

go= co(goGenerator);

// 模仿了著名的框架co，通过co都把这个yield自动当作await去执行了。


async generator可以用来产出一些异步的迭代器，async generator对应于for await of语法

在同步代码中很难见到while(true)，但在异步代码中很常见，业务需求比如写一个表盘，操作系统也会用while(true)去处理。