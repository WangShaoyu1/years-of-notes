Promise对象被写进ES6的规范当中，提供的是另外一种更加友好的对于异步编程的解决方案，在这之前大多使用的是回调函数和事件来实现异步编程。

        怎么来理解Promise对象呢？对于这个ES6新加入的小伙伴，首先是它一个和其他诸如Array，Date，Math，Global等对象一样的东西，只不过Promise可以作为异步编程的一种解决方案，至于是不是最好的异步编程解决方案，这个以后再讨论。
        正因为是为了解决异步编程，相应的，Promise对象是有状态的，事儿是办好了还是办不好，总要有个状态来标识。Promise对象的状态是通过在执行new Promse()的时候传入的两个参数（函数）的执行来决定的，如下：
       
const promise=new Promise((resolve,reject)=>{
    if(.....)
       resolve(argv1)
    else
       reject(argv2)
})


上述例子当中，resolve，reject表示的是promise对象可能有的两种状态，分别表示”成功了“和”失败了“，当然还有一种状态是”进行中“，这个在Promise对象中没办法明显表示出来。总结就是：
Promise对象有三种状态：resolve（完成了，可以了），reject（对不起，报错了），pending（还在酝酿，别着急）这三种状态。
这三种状态怎么来的呢？
就看上述传入到new Promise（）中的参数，（第一个参数表示状态已经完成，第二个表示状态已经失败），哪一个函数先执行了，resolve函数先执行，好了，生成的Promise对象的状态就是已经完成；reject函数先执行，欧了，生成的Promise对象的状态就是失败了，其余的状态就是 进行中。
注意：1.Promise对象的状态一旦决定了，就不能改变了
           2.Promise对象的状态转变只能是两种方式：pending——>resolve（进行中---->完成了）；pending——>reject（进行中----->失败了），再无其他可能性
说好的异步编程呢，哪去了？别急，下文就是就是异步的内容了。
光生成一个Promise对象，只是一个前奏。Promise对象prototype链上有两个方法then（），catch（）；then（）方法表示的是“对象都生成了，然后干啥呢”，这一步是异步操作then方法里面有两个函数参数，和生成Promise对象类似，
then（）和catch（）方法中的参数，就是生成Promsie对象时候，执行resolve和reject方法时传入的参数。
代码如下：

then((resolve)=>{
    //do something
    //Promise对象状态为[b]完成了[/b]的时候执行
},(reject)=>{
     //do something
    //Promise对象状态为[b]失败了[/b]的时候执行
})

但是不推荐以上这种写法，推荐如下写法：

then((resolve)=>{
    //do something
    //Promise对象状态为[b]完成了[/b]的时候执行
}).catch((reject)=>{
    //do something
    //Promise对象状态为[b]失败了[/b]的时候执行
})

铺垫了这么多，上面试题：
题目1：

console.log(1);
new Promise(function (resolve, reject){
    reject(true);
    window.setTimeout(function (){
        resolve(false);
    }, 0);
}).then(function(){
    console.log(2);
}, function(){
    console.log(3);
});
console.log(4);

答案是1 4 3
其中涉及到的知识点有：
1.new Promise 对象的时候，参数函数体内部，先执行了第二个参数（是一个函数），因此生成的Promise对象的状态为是失败了，再无变成其他状态的可能性；
2.在Promise对象生成后，执行的then方法时，由于Promise对象的状态为“失败了”，所以执行then中的第二个参数方法；
3.then方法是异步操作，在输出结果的时候要先等到所有同步操作执行完毕以后再执行，所以，按顺序先执行console.log(1)，console.log(4)，然后再执行console.log(3)

题目2：

new Promise((resolve,reject)=>{
    var bool=true;
    if(bool){
        resolve("yes,good")
    }else{
        reject("no,bad")
    }
}).then((msg)=>{
    console.log("msg:",msg);
    console.log("unknown:",unknown)
}).catch((err)=>{
    console.log("error:",err)
}).then((msg1)=>{
    console.log("msg1:",msg1)
}).then(()=>{
   return "我不是undefined"
).then((msg)=>{
    console.log("msg:",msg);
).then(()=>{
    console.log("到底线了")
})


答案是：msg: yes,good
             error: ReferenceError: unknown is not defined(…)
             msg1: undefined
             msg: 我不是undefined
             到底线了
其中涉及到的知识点有：
1.then方法中的两个回调函数中的参数是生成Promise对象的时候传入的，所以第一个then方法中的msg的实参是"yes,good"；
2.catch会捕捉其之前出现的所有错误，也就是说前面的promise中出现的错误会一直冒泡到后面，直到有catch来接收；
3.then,catch方法执行后会返回一个处理过的Promsie对象，这也是链式写法的基础，至于这个处理后的Promise对象到底是哪个，可以这么理解：
    a.如果then没有返回值，那么返回的是是一个空的Promise对象，由Promise.resolve()或者Promise.reject()直接生成
    b.如果then有返回值data，那么返回的就是Promise.resolve(data)的Promise对象，其等效于new Promise((resolve){resolve(data)}),reject的情况类似，这就解释了第二次打印msg的时
       候是有值的，这就是上面个的then返回的Promise对象的resolve方法 的参数，传递给下面的then方法使用


题目3：
var p1 = new Promise((resolve, reject) => {
    setTimeout(function () {
        reject("出错了")
    }, 1000)
});
var p2 = new Promise((resolve, reject) => {
    resolve(p1)
});
p2.then((msg) => {
    console.log("msg:", msg)
}).catch(() => {
    console.log("出错了")
})



答案是：出错了
其中涉及到的知识点有：
1.当resolve中的参数是一个Promise对象的时候，要等到该Promise对象的状态确定后（完成了或是失败了）才能进行下一步，而且其状态决定了下一个Promise对象的状态，
  案例中，p2的状态要等到p1的状态确定后才决定，然而p1的状态是 失败了，p2的状态也是失败的，这点要注意。

关于Promise对象，还有其他API可以研究，比如Promise.all和Promise.race等等。
关于异步编程，Generator函数以及ES7的async函数都是更好的实现方案。

写在最后的话
Javascript，越来越多的新特性，结合工作中遇到的问题原生支持了越来越多的属性，越来越强大，对于企业级应用开发更加得心应手。
大前端，上手简单，提升阶段需要的知识结构可以说比Java,c++ 等语言更多，更杂；
只是绘制页面，对于前端来说是很基础的工作，结合工作需要，更多的要考虑组件化，工程化，优化性能，接手后台功能；
已经有不少性能稳定的自动化后端服务平台，不需要后端童鞋来配合，后端模块可以按需随意调用，服务器直接用稳定的云服务，更多的工作交给了前端，体验，用户体验，干掉后端不敢说，后端的工作慢慢在弱化；
前端江湖，各种插件，各种组件库风起云涌，快速迭代，开发效率越来越高，这是不是有点像战国时期百家争鸣，民国时期的百花齐放。
这是前端最好的时代，将来可能是Javascript一统江湖的时代（不是说其他语言不存在了，而是说Javascript一支独大）；
胡乱预测，五年后再看这段话。
共勉。

 

