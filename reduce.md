## 数组中的reduce

- 用途：用于对数组进行条件统计，条件求和，筛选求和。

- 一个 “reducer” 函数，包含四个参数：
  - `previousValue`：上一次调用 `callbackFn` 时的返回值。在第一次调用时，若指定了初始值 `initialValue`，其值则为 `initialValue`，否则为数组索引为 0 的元素 `array[0]`。
  - `currentValue`：数组中正在处理的元素。在第一次调用时，若指定了初始值 `initialValue`，其值则为数组索引为 0 的元素 `array[0]`，否则为 `array[1]`。
  - `currentIndex`：数组中正在处理的元素的索引。若指定了初始值 `initialValue`，则起始索引号为 0，否则从索引 1 起始。
  - `array`：用于遍历的数组。
- `initialValue` 可选：

作为第一次调用 `callback` 函数时参数 *previousValue* 的值。若指定了初始值 `initialValue`，则 `currentValue` 则将使用数组第一个元素；否则 `previousValue` 将使用数组第一个元素，而 `currentValue` 将使用数组第二个元素。

- 返回值：

使用 “reducer” 回调函数遍历整个数组后的结果。

 示例：

1. 求数组所有值的和

   ```js
   let sum = [0, 1, 2, 3].reduce(function (previousValue, currentValue) {
     return previousValue + currentValue
   }, 0)
   // sum is 6
   ```

   写成箭头函数

   ```js
   let total = [ 0, 1, 2, 3 ].reduce(
     ( previousValue, currentValue ) => previousValue + currentValue,
     0
   )
   ```

2. 累加对象数组里的值

   要进行累加对象数组中的值，必须提供 initialValue，以便各个 item 正确通过你的函数。

   ```js
   let initialValue = 0
   let sum = [{x: 1}, {x: 2}, {x: 3}].reduce(function (previousValue, currentValue) {
       return previousValue + currentValue.x
   }, initialValue)
   
   console.log(sum) // logs 6
   ```

   写成箭头函数

   ```js
   let initialValue = 0
   let sum = [{x: 1}, {x: 2}, {x: 3}].reduce(
       (previousValue, currentValue) => previousValue + currentValue.x
       , initialValue
   )
   
   console.log(sum) // logs 6
   ```

3. 将二维数组转为一维数组

   concat() 用于连接两个或多个数组

   ```js
   let flattened = [[0, 1], [2, 3], [4, 5]].reduce(
     function(previousValue, currentValue) {
       return previousValue.concat(currentValue)
     },
     []
   )
   // flattened is [0, 1, 2, 3, 4, 5]
   ```

   写成箭头函数

   ```js
   let flattened = [[0, 1], [2, 3], [4, 5]].reduce(
     ( previousValue, currentValue ) => previousValue.concat(currentValue),
     []
   )
   ```

4. 计算数组中每个元素出现的次数

   ```js
   let names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice']
   
   let countedNames = names.reduce(function (allNames, name) {
     if (name in allNames) {
       allNames[name]++
     }
     else {
       allNames[name] = 1
     }
     return allNames
   }, {})
   // countedNames is:
   // { 'Alice': 2, 'Bob': 1, 'Tiff': 1, 'Bruce': 1 }
   ```

5. 按属性对 object 分类

   ```js
   let people = [
     { name: 'Alice', age: 21 },
     { name: 'Max', age: 20 },
     { name: 'Jane', age: 20 }
   ];
   
   function groupBy(objectArray, property) {
     return objectArray.reduce(function (acc, obj) {
       let key = obj[property]
       if (!acc[key]) {
         acc[key] = []
       }
       acc[key].push(obj)
       return acc
     }, {})
   }
   
   let groupedPeople = groupBy(people, 'age')
   // groupedPeople is:
   // {
   //   20: [
   //     { name: 'Max', age: 20 },
   //     { name: 'Jane', age: 20 }
   //   ],
   //   21: [{ name: 'Alice', age: 21 }]
   // }
   ```

6. 使用扩展运算符和 initialValue 绑定包含在对象数组中的数组

   ```js
   // friends - an array of objects
   // where object field "books" is a list of favorite books
   let friends = [{
     name: 'Anna',
     books: ['Bible', 'Harry Potter'],
     age: 21
   }, {
     name: 'Bob',
     books: ['War and peace', 'Romeo and Juliet'],
     age: 26
   }, {
     name: 'Alice',
     books: ['The Lord of the Rings', 'The Shining'],
     age: 18
   }]
   
   // allbooks - list which will contain all friends' books +
   // additional list contained in initialValue
   let allbooks = friends.reduce(function(previousValue, currentValue) {
     return [...previousValue, ...currentValue.books]
   }, ['Alphabet'])
   
   // allbooks = [
   //   'Alphabet', 'Bible', 'Harry Potter', 'War and peace',
   //   'Romeo and Juliet', 'The Lord of the Rings',
   //   'The Shining'
   // ]
   ```

7. 数组去重

   **备注：** 如果你正在使用一个可以兼容[`Set`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set) 和 [`Array.from()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/from) 的环境，你可以使用`let arrayWithNoDuplicates = Array.from(new Set(myArray))` 来获得一个相同元素被移除的数组。 

   ```js
   let myArray = ['a', 'b', 'a', 'b', 'c', 'e', 'e', 'c', 'd', 'd', 'd', 'd']
   let myArrayWithNoDuplicates = myArray.reduce(function (previousValue, currentValue) {
     if (previousValue.indexOf(currentValue) === -1) {
       previousValue.push(currentValue)
     }
     return previousValue
   }, [])
   
   console.log(myArrayWithNoDuplicates)
   ```

8. 使用 .reduce() 替换 .filter().map()

   使用 [`Array.filter()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) 和 [`Array.map()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map) 会遍历数组两次，而使用具有相同效果的 [`Array.reduce()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce) 只需要遍历一次，这样做更加高效。（如果你喜欢 `for` 循环，你可用使用 [`Array.forEach()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) 以在一次遍历中实现过滤和映射数组） 

   ```js
   const numbers = [-5, 6, 2, 0];
   
   const doubledPositiveNumbers = numbers.reduce((previousValue, currentValue) => {
     if (currentValue > 0) {
       const doubled = currentValue * 2;
       previousValue.push(doubled);
     }
     return previousValue;
   }, []);
   
   console.log(doubledPositiveNumbers); // [12, 4]
   ```

9. 按顺序执行 Promise

   ```js
   /**
    * Runs promises from array of functions that can return promises
    * in chained manner
    *
    * @param {array} arr - promise arr
    * @return {Object} promise object
    */
   function runPromiseInSequence(arr, input) {
     return arr.reduce(
       (promiseChain, currentFunction) => promiseChain.then(currentFunction),
       Promise.resolve(input)
     )
   }
   
   // promise function 1
   function p1(a) {
     return new Promise((resolve, reject) => {
       resolve(a * 5)
     })
   }
   
   // promise function 2
   function p2(a) {
     return new Promise((resolve, reject) => {
       resolve(a * 2)
     })
   }
   
   // function 3  - will be wrapped in a resolved promise by .then()
   function f3(a) {
    return a * 3
   }
   
   // promise function 4
   function p4(a) {
     return new Promise((resolve, reject) => {
       resolve(a * 4)
     })
   }
   
   const promiseArr = [p1, p2, f3, p4]
   runPromiseInSequence(promiseArr, 10)
     .then(console.log)   // 1200
   ```

10. 使用函数组合实现管道

    ```js
    // Building-blocks to use for composition
    const double = x => x + x
    const triple = x => 3 * x
    const quadruple = x => 4 * x
    
    // Function composition enabling pipe functionality
    const pipe = (...functions) => initialValue => functions.reduce(
        (acc, fn) => fn(acc),
        initialValue
    )
    
    // Composed functions for multiplication of specific values
    const multiply6 = pipe(double, triple)
    const multiply9 = pipe(triple, triple)
    const multiply16 = pipe(quadruple, quadruple)
    const multiply24 = pipe(double, triple, quadruple)
    
    // Usage
    multiply6(6)   // 36
    multiply9(9)   // 81
    multiply16(16) // 256
    multiply24(10) // 240
    ```

11. 使用 reduce 实现 map

    ```js
    if (!Array.prototype.mapUsingReduce) {
      Array.prototype.mapUsingReduce = function(callback, initialValue) {
        return this.reduce(function(mappedArray, currentValue, currentIndex, array) {
          mappedArray[currentIndex] = callback.call(initialValue, currentValue, currentIndex, array)
          return mappedArray
        }, [])
      }
    }
    
    [1, 2, , 3].mapUsingReduce(
      (currentValue, currentIndex, array) => currentValue + currentIndex + array.length
    ) // [5, 7, , 10]
    ```

    