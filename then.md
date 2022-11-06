## .then() 方法

简介

Promise API 提出以下建议：

每个异步任务都将返回一个 promise 对象。每个 promise 对象都有一个 then 函数，其带两个参数。一个 success 正确处理程序和一个 error 错误处理程序。then 函数中的成功或错误处理仅在调用一次，且在异步任务完成后进行调用。then 功能也将返回 promise。



Promise API旨在解决此嵌套问题和错误处理问题.

Promise API提出以下建议:

1. 每个异步任务都将返回一个`promise`对象.
2. 每个`promise`对象都有一个`then`可以带两个参数的`success` 函数,一个`error`处理程序和一个处理程序.
3. 在异步任务完成后,函数中的成功或错误处理程序`then`将仅被调用*一次*.
4. 该`then`函数还将返回a `promise`,以允许链接多个调用.
5. 每个处理程序(成功或错误)都可以返回一个`value`,它将`argument`在`promise`s 链中作为a传递给下一个函数.
6. 如果处理程序返回a `promise`(发出另一个异步请求),那么只有在该请求完**成后才会调用下一个处理程序(成功或错误).**

**1.Promise.prototype.then()方法** 

Promise 实例具有 then 方法，也就是说：then() 方法是定义在原型对象 Promise.prototype 上的。它的作用是为 Promise 实例添加状态改变时的回调函数。then方法的第一个参数是resolved状态的回调函数，第二个参数（可选）是rejected状态的回调函数。 

then方法返回的是一个新的Promise实例（注意，不是原来那个Promise实例）。因此可以采用链式写法，即then方法后面再调用另一个then方法。 【then()之所以可以进行链式回调是因为上一个then的成功回调依旧是一个promise对象】

案例一：

```js
getJSON("/posts.json").then(function(json) {
return json.post;
}).then(function(post) {
// ...
});
```

上面的代码使用then方法，依次指定了两个回调函数。第一个回调函数完成以后，会将返回结果作为参数，传入第二个回调函数。

采用链式的then，可以指定一组按照次序调用的回调函数。这时，前一个回调函数，有可能返回的还是一个Promise对象（即有异步操作），这时**后一个回调函数，就会等待该Promise对象的状态发生变化，才会被调用**。

案例二：

```js
getJSON("/post/1.json").then(function(post) {
return getJSON(post.commentURL);
}).then(function funcA(comments) {
console.log("resolved: ", comments);
}, function funcB(err){
console.log("rejected: ", err);
});
```

上面代码中，第一个then方法指定的回调函数，返回的是另一个Promise对象。这时，第二个then方法指定的回调函数，就会等待这个新的Promise对象状态发生变化。如果变为resolved，就调用funcA，如果状态变为rejected，就调用funcB。

如果采用箭头函数，上面的代码可以写得更简洁。

```js
getJSON("/post/1.json").then(
post => getJSON(post.commentURL)
).then(
comments => console.log("resolved: ", comments),
err => console.log("rejected: ", err)
);
```

## .catch() 方法

**2.Promise.prototype.catch()方法** 

Promise.prototype.catch方法是.then(null, rejection)的别名，用于指定发生错误时的回调函数。 

```js
getJSON('/posts.json').then(function(posts) {
// ...
}).catch(function(error) {
// 处理 getJSON 和 前一个回调函数运行时发生的错误
console.log('发生错误！', error);
});
```

上面代码中，getJSON方法返回一个 Promise 对象，如果该对象状态变为**resolved**，则会调用then方法指定的回调函数；如果异步操作抛出错误，状态就会变为**rejected**，就会调用catch方法指定的回调函数，处理这个错误。另外，then方法指定的回调函数，如果运行中抛出错误，也会被catch方法捕获。 

总结示例：

```js
p.then((val) => console.log('fulfilled:', val))
.catch((err) => console.log('rejected', err));
// 等同于
p.then((val) => console.log('fulfilled:', val))
.then(null, (err) => console.log("rejected:", err));
```

## catch()方法的使用意义

先知道：

- reject 是用来抛出异常的，catch 是用来处理异常的 
- reject 是 Promise 的方法，而 then 和 catch 是 Promise 实例的方法（Promise.prototype.then 和 Promise.prototype.catch） 

区别：

主要区别就是，如果在then的第一个函数里抛出了异常，后面的catch能捕获到，而then的第二个函数捕获不到。 

**catch只是一个语法糖而己 还是通过then 来处理的，大概如下所示字体：**

```js
Promise.prototype.catch = function(fn){
    return this.then(null,fn);
}
```









**建议总是使用catch方法，而不使用then方法的第二个参数。**

为啥？

因为

```js
getJSON('/post/1.json').then(function(post) {
return getJSON(post.commentURL);
}).then(function(comments) {
// some code
}).catch(function(error) {
// 处理前面三个Promise产生的错误
});
```

上面代码中，一共有三个 Promise 对象：一个由getJSON产生，两个由then产生。它们之中任何一个抛出的错误，都会被最后一个catch捕获。 一般来说，不要在then方法里面定义 Reject 状态的回调函数（即then的第二个参数），总是使用catch方法 

## 总结

**建议总是使用catch方法，而不使用then方法的第二个参数。** 

**因为 Promise 的状态一旦改变，就永久保持该状态，不会再变了。** 

**Promise 对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。也就是说，错误总是会被下一个catch语句捕获** 

1、Promise.prototype.then()方法显然就是Promise的精华。函数声明：p.then(resolve, reject);。
then()方法不是静态方法，需要经由Promise实例对象来调用。
then方法有两个参数，第一个参数是Promise实例对象为Resolved状态时的回调函数，它的参数就是上面Promise构造函数里resolve传递过来的异步操作成功的结果。

第二个参数可选，是Promise实例对象为Rejected状态时的回调函数，它的参数就是上面Promise构造函数里reject传递过来的异步操作失败的信息。

then方法最强大之处在于，它内部可以使用return或throw来实现链式调用。使用return或throw后的返回值是一个新的Promise实例对象（注意，不是原来那个Promise实例对象）：

2、Promise.prototype.catch()同样是实例方法，需要经由Promise实例对象来调用，用于Promise实例对象状态为Rejected的后续处理，即异常处理。函数声明：p.catch(reject);

catch方法本质上等价于then(null, reject)，参数reject是一个回调函数，它的参数就是Promise对象状态变为Rejected后，传递来的错误信息。