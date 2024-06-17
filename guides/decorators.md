# Decorators

A **decorator**

- is a function which, when applied to a given function, gives the given function new capabilities.

Examples of new capabilities include:

- Resource measurement
- Caching
- Throttling

## A basic "decorator"

```js
functionA = (n) => {
  console.log("here is n", n)
}

functionB = (func) => {
  console.log("I am functionB")
  func()
}
```

## A real decorator

But this does not allow us to pass any arguments! How can we give the function new functionality AND keep the old? A true decorator will allow us to do that by writing an inner wrapping function which takes any number of arguments, using this to call the given function. This inner wrapping function is then the return value of our decorator. Consider this example:

```js
const decorator = (fn) => {
  let wrap = (...args) => {
    console.log("wrapping:", fn)
    let out = fn.apply(this, args)
    return out
  }
  return wrap
}
```

The `wrap` function has an arbitrary function signature, that is, one compatible with **any** function. It prints a log and then calls the original function with the arguments passed to it. The `decorator` function returns `wrap` and then the idea is to use `wrap` in place of `fn`. For example:

```js
> const fn = (n) => (n*(n+1))/2
> fn(10)
55
> const wrapFn = decorator(fn)
> wrapFn(10)
wrapping: (n) => (n*(n+1))/2
55
```

## The `time` decorator

Our decorator example before allowed us to wrap a function and do what we like with it. What can we do with this ability? We can measure how long function calls take using the following decorator:

```js
const time = (fn) => {
  let timed = (...args) => {
    let start = new Date()
    let out = fn.apply(this, args)
    let end = new Date()
    console.log("Ran in", end-start, "ms")
    return out
  }
  return timed
}
```

As a specific example, here is a function that adds the numbers 1 to `n` naively:

```js
const naive = n => {
  let total = 0
  for(let i=0;i<=n;i++) {
    total += i
  }
  return total
}
```

This function is problematic for a couple of reasons, in particular that it gives the wrong answers when the sums become larger than `Number.MAX_SAFE_INTEGER`, but beyond that it also starts getting slower. Try running `naive(1e9)` to see how long it takes. We can use the `time` decorator to measure this effect.

```js
timeNaive(1e9)
Ran in 915 ms
500000000067109000
```

This code should bother you! It is synchronous code, but it takes a nonzero amount of time. That means that during its execution it is **blocking** any other execution. This is a problem, and we still want timing information, so we can adjust the `time` decorator to be asynchronous as follows:

```js
const timeAsync = (fn) => {
  let timed = async (...args) => {
    let start = new Date()
    let out = await fn.apply(this, args)
    let end = new Date()
    console.log("Ran in", end-start, "ms")
    return out
  }
  return timed
}
```

Then we can run the same test again:

```js
> timeAsyncNaive = timeAsync(naive)
> timeAsyncNaive(1e9)
Ran in 942 ms
Promise {<resolved>: 500000000067109000}
```

The value returned is now a `Promise` and can be used with the usual `async` and `await` machinery.

We can further test this decorator with a function designed to sleep for a specified amount of time:

```js
const sleep = async (time) => {
  console.log(`Sleeping ${time} ms`)
  await new Promise((resolve) => setTimeout(resolve, time))
  console.log("Waking up!")
}
```

Now we can use `sleepAsync` to demonstrate how `time` works:

```js
> const timeSleep = timeAsync(sleep)
> timeSleep(1000)
Sleeping 1000 ms
Waking up!
Ran in 1003 ms
```

## The `memoize` decorator

Memoization is a great way to cache function calls according to their arguments. This technique trades space for time, and as a result can make your code run faster. Here is a sample `memoize` decorator:

```js
const memoize = (fn, keymaker = JSON.stringify) => {
  const lookupTable = new Map()

  return (...args) => {
    const key = keymaker(args)
    return lookupTable[key] || (lookupTable[key] = fn(...args))
  }
}
```

We can use our old friend `fibonacci` to test this decorator:

```js
const fibonacci = (n) => {
  if(n === 1 || n === 2) return 1
  return fibonacci(n-1) + fibonacci(n-2)
}
```

Making use of the `time` decorator we can see a stark difference:

```js
> timeMemFib = time(memoize(fibonacci))
> timeMemFib(40)
Ran in 970 ms
102334155
> timeMemFib(40)
Ran in 1 ms
102334155
```

Unfortunately, this is not ideal for our `fibonacci` function, as it only memoizes the final answer, so it will still take a significant amount of time to compute `fibonacci(39)` or `fibonacci(41)`.

```js
> timeMemFib(41)
Ran in 1598 ms
165580141
> timeMemFib(41)
Ran in 0 ms
165580141
```

This can be solved in several ways, mutating a global, using a closure, but some of these ways are not "functional" in the sense of being side-effect free. Even so, there are ways to construct a side-effect free memoization. One such example known as parametrization, and an example is included as an appendix.

## Once decorator

Run a function exactly once. See also the [Singleton](https://en.wikipedia.org/wiki/Singleton_pattern) design pattern.

```js
const once = (fn) => {
  let returnValue
  let canRun = true
  return (...args) => {
      if(canRun) {
          returnValue = fn(...args)
          canRun = false
      }
      return returnValue
  }
}
```

## Throttle decorator

Call a function only so often. This is useful for limiting external API access, or making sure the client does not crash because they spam an action.

```js
const throttle = (fn, interval) => {
    let lastTime
    return (...args) => {
        let timeSinceLastExecution = Date.now() - lastTime
        if(!lastTime || (timeSinceLastExecution >= interval)) {
            fn(...args)
            lastTime = Date.now()
        }
    }
}
```

## After decorator

This decorator calls a function, but only after it has been called a certain number of times previously.

```js
const after = (count, fn) => {
   let runCount = 0
   return (...args) => {
      runCount = runCount + 1
      if (runCount >= count) {
         return fn(...args)
      }
   }
}
```

## Debounce decorator

This decorator allows a function to be called only when it has not been called in the last `interval` amount of time.

```js
const debounce = (fn, interval) => {
    let timer
    return (...args) => {
        clearTimeout(timer)
        timer = setTimeout(() => fn(...args), interval)
    };
}
```

## Curry decorator

This decorator curries a function.

```js
const curry = fn => (
  (...args) => (
    args.length >= fn.length ?
      fn(...args) :
      curry(fn.bind(null, ...args))
  )
)
```

## Appendix: Fast Functional Faultless Fibonacci

**Note** Faultless is only guaranteed insofar as this approach will yield correct answers. Any mistakes leading to incorrect answers are those of the author of this document.

This entirely functional memoization approach to the Fibonacci problem is known as parametrization since now the function itself (`fibonacciParam`) is a parameter. Once this function is memoized, each recursive call takes advantage of the memo table, making this approach blazing fast.

```js
> const fibonacciParam = (fibonacci, n) => {
  if(n === 1 || n === 2) return 1
  return fibonacci(fibonacci, n-1) + fibonacci(fibonacci, n-2)
}
> const memFibParam = memoize(fibonacciParam)
> const memoizedFibonacci = (n) => memFibParam(memFibParam, n)
> const timeMemoizedFibonacci = timeAsync(memoizedFibonacci)
> timeMemoizedFibonacci(78)
Ran in 0 ms
Promise {<resolved>: 8944394323791464}
```

As you can see, this is *VERY* fast, but now we have a bigger problem:

```js
> timeMemoizedFibonacci(79)
Ran in 0 ms
Promise {<resolved>: 14472334024676220}
```

But the 79th Fibonacci number is actually `14472334024676221`. Our function gives the wrong answer because it is larger than the integer precision that Javascript supports by default.

```js
> 14472334024676221 > Number.MAX_SAFE_INTEGER
true
```

This is a problem in core Javascript, but it can be mitigated with an arbitrary precision integer arithmetic library. Fortunately, there is a [proposal](https://github.com/tc39/proposal-bigint) (as of now in stage 3 of 4) for the `BigInt` object which introduces arbitrary precision integer arithmetic into core Javascript. Below is some code which uses this proposal:

```js
const bigIntFibonacciParam = (fibonacci, n) => {
  if(n === 1 || n === 2) return BigInt(1)
  return fibonacci(fibonacci, n-1) + fibonacci(fibonacci, n-2)
}
const memBigIntFibParam = memoize(bigIntFibonacciParam)
const memBigIntFibonacci = (n) => memBigIntFibParam(memBigIntFibParam, n)
const timeMemBigIntFibonacci = timeAsync(memBigIntFibonacci)
```

With these changes, we can easily calculate the 78th and 79th, or even the 100th  Fibonacci number!

```js
> timeMemBigIntFibonacci(78)
Ran in 0 ms
Promise {<resolved>: 8944394323791464n}
> timeMemBigIntFibonacci(79)
Ran in 0 ms
Promise {<resolved>: 14472334024676221n}
> timeMemBigIntFibonacci(100)
Ran in 0 ms
Promise {<resolved>: 354224848179261915075n}
```

Actually, this method doesn't even break a sweat on the 1000th Fibonacci number:

```js
> timeMemBigIntFibonacci(1000)
Ran in 7 ms
Promise {<resolved>: 43466557686937456435688527675040625802564660517371780402481729089536555417949051890403879840079255169295922593080322634775209689623239873322471161642996440906533187938298969649928516003704476137795166849228875n}
```

Jenkies! That's a big number. You're on your own from here. :heart:
If you are interested in the next challenge, try calculating the [millionth Fibonacci number](Fibonacci-1e6.md).
