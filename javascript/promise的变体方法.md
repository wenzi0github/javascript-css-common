### 1 Promise.first

场景：一个页面当前正处于 loading 状态，同时请求了多个接口，无论哪个接口正确返回结果，则 loading 效果取消！或者其他的要获取获取第一个完成状态的值。

这里就要用到了`Promise.first`了，只要任意一个 Promise 实例变成完成状态，则 Promise.first 变成完成状态。其实这里并不适合`Promise.race`方法，因为第一个变成拒绝状态的实例也会激活 Promise.race，

```javascript
if (!Promise.first) {
    // get first resolve result
    Promise.first = promiseList => {
        return new Promise((resolve, reject) => {
            var num = 0;
            var len = promiseList.length;
            promiseList.forEach(pms => {
                Promise.resolve(pms)
                    .then(resolve)
                    .catch(() => {
                        num++;
                        if (num === len) {
                            reject('all promises not resolve');
                        }
                    });
            });
        });
    };
}
```

调用方式：

```javascript
Promise.first([p4_fail_400, p2_suc_500, p3_suc_300])
    .then(res => console.log(res)) // p3-suc-300
    .catch(e => console.error(e));
```

可以看到每次获取的 p3_suc_300 的值，因为 p4 是失败的状态，p2 的完成状态没有 p3 快，因此这里获取到了 p3 的结果。

### 2 Promise.last

与 Promise.first 对应的则是`Promise.last`，获取最后变成完成状态的值。这里与 Promise.first 不同的是，只有最后一个 Promise 都变成最终态（完成或拒绝），才能知道哪个是最后一个完成的，这里我采用了计数的方式，`then`和`catch`只能二选一，等计数器达到 list.length 时，执行外部的 resolve。

```javascript
if (!Promise.last) {
    // get last resolve result
    Promise.last = promiseList => {
        return new Promise((resolve, reject) => {
            let num = 0;
            let len = promiseList.length;
            let lastResolvedResult;

            const fn = () => {
                if (++num === len) {
                    lastResolvedResult
                        ? resolve(lastResolvedResult)
                        : reject('all promises rejected');
                }
            };
            promiseList.forEach(pms => {
                Promise.resolve(pms)
                    .then(res => {
                        lastResolvedResult = res;
                        fn();
                    })
                    .catch(fn);
            });
        });
    };
}
```

调用方式：

```javascript
Promise.last([p1_suc_100, p2_suc_500, p5_fail_200, p3_suc_300, p4_fail_400])
    .then(res => console.log(res)) // p2-suc-500
    .catch(e => console.error(e));
```

p2 需要 500ms 才能完成，是最晚完成的。

### 3 Promise.none

`Promise.none`与 Promise.all 正好相反，所有的 promise 都被拒绝了，则 Promise.none 变成完成状态。该方法可以用 Promise.first 来切换，当执行 Promise.first 的 catch 时，则执行 Promise.none 中的 resolve。不过这里我们使用 Promise.all 来实现。

```javascript
if (!Promise.none) {
    // if all the promises rejected, then succes
    Promise.none = promiseList => {
        return Promise.all(
            promiseList.map(pms => {
                return new Promise((resolve, reject) => {
                    // 将pms的resolve和reject反过来
                    return Promise.resolve(pms).then(reject, resolve);
                });
            })
        );
    };
}
```

调用方式：

```javascript
Promise.none([p5_fail_200, p4_fail_400])
    .then(res => console.log(res))
    .catch(e => console.error(e));

// then的输出结果：
// [
//     Error: testPromise is error, name: p5-fail-200,
//     Error: testPromise is error, name: p4-fail-400
// ]
```

两个 promise 都失败后，则 Promise.none 进入完成状态。

### 4 Promise.any

Promise.any 表示只获取所有的 promise 中进入完成状态的结果，被拒绝的则忽略掉。

```javascript
if (!Promise.any) {
    // get only resolve the results
    Promise.any = promiseList => {
        let result = [];
        return Promise.all(
            promiseList.map(pms => {
                return Promise.resolve(pms)
                    .then(res => result.push(res))
                    .catch(e => {});
            })
        ).then(res => {
            return new Promise((resolve, reject) => {
                result.length ? resolve(result) : reject();
            });
        });
    };
}
```

调用方式：

```javascript
Promise.any([p1_suc_100, p2_suc_500, p5_fail_200, p3_suc_300, p4_fail_400])
    .then(res => console.log(res)) // ["p1-suc-100", "p3-suc-300", "p2-suc-500"]
    .catch(e => console.error(e));
```

### 5 Promise.every

最后一个的实现比较简单，所有的 promise 都进入完成状态，则返回 true，否则返回 false。

```javascript
if (!Promise.every) {
    // get the boolean if all promises resolved
    Promise.every = promiseList => {
        return Promise.all(promiseList)
            .then(() => Promise.resolve(true))
            .catch(() => Promise.resolve(false));
    };
}
```

调用方式：

```javascript
Promise.every([p1_suc_100, p2_suc_500, p3_suc_300]).then(result =>
    console.log('Promise.every', result)
); // Promise.every true

Promise.every([p1_suc_100, p4_fail_400]).then(result =>
    console.log('Promise.every', result)
); // Promise.every false
```
