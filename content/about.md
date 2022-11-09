---
title: About
---

```js
const asyncPool = async (poolLimit, array, iteratorFn) => {
  const resultList = [];
  const executingList = [];
  for(const item of array) {
    const p = Promise.resolve().then(() => {
	  return iteratorFn(item, array);
    });
    resultList.push(p);
    if (poolLimit <= array.length) {
      const e = p.then(() => {
        return executingList.splice(executingList.indexOf(e), 1);
      });
      executingList.push(e);
      if (executingList.length >= poolLimit) {
        await Promise.race(executingList);
      }
    }
  }
  return Promise.all(resultList);
}
```


`for...of`在async函数里使用时，`for...of`里的`await`关键字会暂停遍历。

`p`是一个promise，它的完成时机取决于`iteratorFn`返回的promise。

3. `e`是一个promise，它的作用是，等到`p`成功以后，那么把这个任务从`executingList`中移出。
4. `executingList.length >= poolLimit`时，这块比较绕，举个例子吧，如果我们的`poolLimit`值为5，那么当我们遍历到第五个item的时候，我们不能继续无脑地接着遍历后面的item了，而是要等到当前`executingList`里的某个任务结束了，再接着遍历。
5. `await Promise.race(executingList)`的意义是，一旦`executingList`中的任一个任务成功，那么才继续遍历。这种写法很牛逼。
6. 等到所有的`for...of`都结束了，才会执行`return Promise.all(resultList)`，返回所有的结果。

总结：
上面的这种实现方式真高级，也很绕，如果对async函数，for...of，Promise掌握得不好，甚至看不懂。
