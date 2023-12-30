# LeetCode
# 2623. Memoize

Given a function fn, return a memoized version of that function.

A memoized function is a function that will never be called twice with the same inputs. Instead it will return a cached value.

You can assume there are 3 possible input functions: sum, fib, and factorial.

sum accepts two integers a and b and returns a + b.
fib accepts a single integer n and returns 1 if n <= 1 or fib(n - 1) + fib(n - 2) otherwise.
factorial accepts a single integer n and returns 1 if n <= 1 or factorial(n - 1) * n otherwise.






### Intuition
I understood by reading problem that args should not be same next time whenever function is called next time.

### Approach
Now the main problem arises how and what should  I do so that args passed would not be same so chatgpt it and find a way to put in a key by converting them into string. Now applied conditional statement and answer is here.

### Complexity
- Space Complexity: O(n) where n is the number of unique argument combinations.
- Time Complexity: O(n) for key generation, O(1) for cache access, and depends on the original function for function computation.


## Code
```javascript
/**
 * @param {Function} fn
 * @return {Function}
 */
function memoize(fn) {
    let cache ={}
    return function(...args) {
      const key=JSON.stringify(args) //problem-solver 
      if (cache[key]===undefined){
          return cache[key]=fn(...args)
      }
      return cache[key]
    }
}


/** 
 * let callCount = 0;
 * const memoizedFn = memoize(function (a, b) {
 *	 callCount += 1;
 *   return a + b;
 * })
 * memoizedFn(2, 3) // 5
 * memoizedFn(2, 3) // 5
 * console.log(callCount) // 1 
 */
```
# 2723. Add Two Promises

Given two promises promise1 and promise2, return a new promise. promise1 and promise2 will both resolve with a number. The returned promise should resolve with the sum of the two numbers.
 

Example 1:

Input: 
promise1 = new Promise(resolve => setTimeout(() => resolve(2), 20)), 
promise2 = new Promise(resolve => setTimeout(() => resolve(5), 60))
Output: 7
Explanation: The two input promises resolve with the values of 2 and 5 respectively. The returned promise should resolve with a value of 2 + 5 = 7. The time the returned promise resolves is not judged for this problem.


Example 2:

Input: 
promise1 = new Promise(resolve => setTimeout(() => resolve(10), 50)), 
promise2 = new Promise(resolve => setTimeout(() => resolve(-12), 30))
Output: -2
Explanation: The two input promises resolve with the values of 10 and -12 respectively. The returned promise should resolve with a value of 10 + -12 = -2.
# Intuition
I felt that its gonna be 1-2 line solution but how what I didnt know.

# Approach
Simply, go to chatgpt understood problem and when I understood problem comeback to make logic and code but failed then go to discussion and found someone told about Promise.all and the answer is here

# Complexity
- Time Complexity: O(1)
- Space Complexity: O(1)

# Code
```javascript
/**
 * @param {Promise} promise1
 * @param {Promise} promise2
 * @return {Promise}
 */
var addTwoPromises = async function(promise1, promise2) {
    return Promise.all([promise1,promise2]).then((values=>{  //GameChanger 
        const sum=values[0]+values[1] //const sum = values.reduce((acc, value) => acc + value);
        return sum
    }))
};

/**
 * addTwoPromises(Promise.resolve(2), Promise.resolve(2))
 *   .then(console.log); // 4
 */
```
# 2621. Sleep

Given a positive integer millis, write an asynchronous function that sleeps for millis milliseconds. It can resolve any value.

 

Example 1:

Input: millis = 100
Output: 100
Explanation: It should return a promise that resolves after 100ms.
let t = Date.now();
sleep(100).then(() => {
  console.log(Date.now() - t); // 100
});
Example 2:

Input: millis = 200
Output: 200
Explanation: It should return a promise that resolves after 200ms.

### Intuition
I have make a function which returns after miilis time which is a positive interger.

### Approach
Settimeout is used with Promise to resolve the problem

### Complexity
- Time complexity:
O(1)

- Space complexity:
O(1)

## Code
```javascript
/**
 * @param {number} millis
 * @return {Promise}
 */
async function sleep(millis) {
return new  Promise(resolve=>{
    setTimeout(()=>{
        resolve("message will print after millis")
    },millis)
})
}

/** 
 * let t = Date.now()
 * sleep(100).then(() => console.log(Date.now() - t)) // 100
 */
```

# 2715. Timeout Cancellation

Given a function fn, an array of arguments args, and a timeout t in milliseconds, return a cancel function cancelFn.

After a delay of cancelTimeMs, the returned cancel function cancelFn will be invoked.

setTimeout(cancelFn, cancelTimeMs)
Initially, the execution of the function fn should be delayed by t milliseconds.

If, before the delay of t milliseconds, the function cancelFn is invoked, it should cancel the delayed execution of fn. Otherwise, if cancelFn is not invoked within the specified delay t, fn should be executed with the provided args as arguments.

 

Example 1:

Input: fn = (x) => x * 5, args = [2], t = 20
Output: [{"time": 20, "returned": 10}]
Explanation: 
const cancelTimeMs = 50;
const result = [];

const fn = (x) => x * 5;

const start = performance.now();

const log = (...argsArr) => {
    const diff = Math.floor(performance.now() - start);
    result.push({"time": diff, "returned": fn(...argsArr)});
}
     
const cancel = cancellable(log, [2], 20);

const maxT = Math.max(t, 50);
          
setTimeout(cancel, cancelTimeMs);

setTimeout(() => {
     console.log(result); // [{"time":20,"returned":10}]
}, maxT + 15);

The cancellation was scheduled to occur after a delay of cancelTimeMs (50ms), which happened after the execution of fn(2) at 20ms.


Example 2:

Input: fn = (x) => x**2, args = [2], t = 100
Output: []
Explanation: 
const cancelTimeMs = 50;
The cancellation was scheduled to occur after a delay of cancelTimeMs (50ms), which happened before the execution of fn(2) at 100ms, resulting in fn(2) never being called.


Example 3:

Input: fn = (x1, x2) => x1 * x2, args = [2,4], t = 30
Output: [{"time": 30, "returned": 8}]
Explanation: 
const cancelTimeMs = 100;
The cancellation was scheduled to occur after a delay of cancelTimeMs (100ms), which happened after the execution of fn(2,4) at 30ms.

## Pre-Requisite



### Callback Functions:
A callback function is a function that is passed as an argument to another function and is executed after the completion of a specific task or event. Callbacks are commonly used in asynchronous programming, where functions are not executed immediately but are scheduled to run later. They are crucial for handling tasks like handling events, making asynchronous API calls, and managing timeouts.

Example of a callback function:

```javascript
function doSomethingAsync(callback) {
  // Simulating an asynchronous operation
  setTimeout(function () {
    console.log("Task completed!");
    callback();
  }, 1000);
}

function onComplete() {
  console.log("Callback function executed!");
}

doSomethingAsync(onComplete);
```

### Rest Parameters:
Rest parameters allow you to represent an indefinite number of arguments as an array. This is useful when you want to create functions that can accept any number of arguments.

Example of rest parameters:

```javascript
function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3, 4)); // Output: 10
```

In the example above, the `...numbers` syntax gathers all the arguments into an array named `numbers`.

### `setTimeout` and `clearTimeout`:

#### `setTimeout`:
The `setTimeout` function is used to schedule the execution of a function after a specified delay (in milliseconds).

Example:

```javascript
function delayedFunction() {
  console.log("Delayed function executed!");
}

setTimeout(delayedFunction, 2000); // Executes delayedFunction after 2000 milliseconds (2 seconds)
```

#### `clearTimeout`:
The `clearTimeout` function is used to cancel a timeout that has been previously set with `setTimeout`. It takes the ID returned by `setTimeout` as an argument.

Example:

```javascript
function delayedFunction() {
  console.log("Delayed function executed!");
}

const timeoutId = setTimeout(delayedFunction, 2000);

// Cancel the timeout before it executes
clearTimeout(timeoutId);
```

In the example above, the `clearTimeout` function is used to prevent the execution of `delayedFunction` if it hasn't already occurred.


### Intuition
basic funda yhi thaa ki timer ko compare hai..

### Approach
chatgpt kiya solutions dekhe bhoot confuse hue pr kr diya dekh daakh k

### Complexity
- Time complexity:
O(1)

- Space complexity:
O(1)

## Code
```javascript
/**
 * @param {Function} fn - The function to be executed after the delay.
 * @param {Array} args - An array of arguments to be passed to the function.
 * @param {number} t - The time delay in milliseconds.
 * @return {Function} - The cancel function.
 */
const cancellable = function (fn, args, t) {
  // Declare a variable to hold the timer ID.
  let timer;

  // Define the cancel function.
  const cancelFn = function () {
    // Check if the timer is set (not yet fired or cleared).
    if (timer) {
      // If the timer is set, clear it.
      clearTimeout(timer);
      // Set the timer variable to null, indicating it's cleared.
      timer = null;
    }
  };

  // Set a timeout to execute the provided function after the specified delay.
  timer = setTimeout(() => {
    // Execute the provided function with the specified arguments.
    fn(...args);
    // Set the timer variable to null after executing the function.
    // This helps in cases where the cancel function is called after the timeout has already occurred.
    timer = null;
  }, t);

  // Return the cancel function.
  return cancelFn;
};
```
# 2725. Interval Cancellation
Given a function fn, an array of arguments args, and an interval time t, return a cancel function cancelFn.

The function fn should be called with args immediately and then called again every t milliseconds until cancelFn is called at cancelTimeMs ms.

 

Example 1:

Input: fn = (x) => x * 2, args = [4], t = 35
Output: 
[
   {"time": 0, "returned": 8},
   {"time": 35, "returned": 8},
   {"time": 70, "returned": 8},
   {"time": 105, "returned": 8},
   {"time": 140, "returned": 8},
   {"time": 175, "returned": 8}
]
Explanation: 
const cancelTimeMs = 190;
const cancelFn = cancellable((x) => x * 2, [4], 35);
setTimeout(cancelFn, cancelTimeMs);

Every 35ms, fn(4) is called. Until t=190ms, then it is cancelled.
1st fn call is at 0ms. fn(4) returns 8.
2nd fn call is at 35ms. fn(4) returns 8.
3rd fn call is at 70ms. fn(4) returns 8.
4th fn call is at 105ms. fn(4) returns 8.
5th fn call is at 140ms. fn(4) returns 8.
6th fn call is at 175ms. fn(4) returns 8.
Cancelled at 190ms
Example 2:

Input: fn = (x1, x2) => (x1 * x2), args = [2, 5], t = 30
Output: 
[
   {"time": 0, "returned": 10},
   {"time": 30, "returned": 10},
   {"time": 60, "returned": 10},
   {"time": 90, "returned": 10},
   {"time": 120, "returned": 10},
   {"time": 150, "returned": 10}
]
Explanation: 
const cancelTimeMs = 165; 
const cancelFn = cancellable((x1, x2) => (x1 * x2), [2, 5], 30) 
setTimeout(cancelFn, cancelTimeMs)

Every 30ms, fn(2, 5) is called. Until t=165ms, then it is cancelled.
1st fn call is at 0ms 
2nd fn call is at 30ms 
3rd fn call is at 60ms 
4th fn call is at 90ms 
5th fn call is at 120ms 
6th fn call is at 150ms
Cancelled at 165ms
Example 3:

Input: fn = (x1, x2, x3) => (x1 + x2 + x3), args = [5, 1, 3], t = 50
Output: 
[
   {"time": 0, "returned": 9},
   {"time": 50, "returned": 9},
   {"time": 100, "returned": 9},
   {"time": 150, "returned": 9}
]
Explanation: 
const cancelTimeMs = 180;
const cancelFn = cancellable((x1, x2, x3) => (x1 + x2 + x3), [5, 1, 3], 50)
setTimeout(cancelFn, cancelTimeMs)

Every 50ms, fn(5, 1, 3) is called. Until t=180ms, then it is cancelled. 
1st fn call is at 0ms
2nd fn call is at 50ms
3rd fn call is at 100ms
4th fn call is at 150ms
Cancelled at 180ms
### Intuition
understood the solution but when called with same method as timeout cancellation then it give an error of time exceeding then I follow solution and found I have to immediately invovke my function 

### Approach

- Call fn(...args).
- Set an interval timer. The setInterval function in the code below will call () => fn(...args) every t milliseconds. Note, setInterval does not initially call the function before t milliseconds, which is why we call fn(...args) once before setting the interval.
- Now, we define a cancelFn function, which clears the interval when called. Return cancelFn.
- The function cancelFn is not called when our cancellable function is first defined. However, whenever someone calls cancellable, the line return cancelFn, in order to return, will call and execute cancelFn, thereby cancelling the interval.
For example, if we define var myFunc = cancellable((num) => 1 + num, 13, 100), the interval will repeatedly call (num) => 1 + num until myFunc() is called. When myFunc() is called, the return line in myFunc is read, which will consequentially make cancelFn execute and return, thereby clearing the interval.

### Complexity
- Time complexity:O(n)


- Space complexity:o(1)



## Code
```javascript
/**
 * @param {Function} fn
 * @param {Array} args
 * @param {number} t
 * @return {Function}
 */
var cancellable = function(fn, args, t) {
    fn(...args);//Immediately invoked thats the solution too
    let timer = setInterval(() => fn(...args), t);

    let cancelFn = () => clearInterval(timer);
    return cancelFn;
};
/**
 *  const result = [];
 *
 *  const fn = (x) => x * 2;
 *  const args = [4], t = 35, cancelTimeMs = 190;
 *
 *  const start = performance.now();
 *
 *  const log = (...argsArr) => {
 *      const diff = Math.floor(performance.now() - start);
 *      result.push({"time": diff, "returned": fn(...argsArr)});
 *  }
 *       
 *  const cancel = cancellable(log, args, t);
 *
 *  setTimeout(cancel, cancelTimeMs);
 *   
 *  setTimeout(() => {
 *      console.log(result); // [
 *                           //     {"time":0,"returned":8},
 *                           //     {"time":35,"returned":8},
 *                           //     {"time":70,"returned":8},
 *                           //     {"time":105,"returned":8},
 *                           //     {"time":140,"returned":8},
 *                           //     {"time":175,"returned":8}
 *                           // ]
 *  }, cancelTimeMs + t + 15)    
 */
```
# 2637. Promise Time Limit
Given an asynchronous function fn and a time t in milliseconds, return a new time limited version of the input function. fn takes arguments provided to the time limited function.

The time limited function should follow these rules:

If the fn completes within the time limit of t milliseconds, the time limited function should resolve with the result.
If the execution of the fn exceeds the time limit, the time limited function should reject with the string "Time Limit Exceeded".
 

Example 1:

Input: 
fn = async (n) => { 
  await new Promise(res => setTimeout(res, 100)); 
  return n * n; 
}
inputs = [5]
t = 50
Output: {"rejected":"Time Limit Exceeded","time":50}
Explanation:
const limited = timeLimit(fn, t)
const start = performance.now()
let result;
try {
   const res = await limited(...inputs)
   result = {"resolved": res, "time": Math.floor(performance.now() - start)};
} catch (err) {
   result = {"rejected": err, "time": Math.floor(performance.now() - start)};
}
console.log(result) // Output

The provided function is set to resolve after 100ms. However, the time limit is set to 50ms. It rejects at t=50ms because the time limit was reached.
Example 2:

Input: 
fn = async (n) => { 
  await new Promise(res => setTimeout(res, 100)); 
  return n * n; 
}
inputs = [5]
t = 150
Output: {"resolved":25,"time":100}
Explanation:
The function resolved 5 * 5 = 25 at t=100ms. The time limit is never reached.
Example 3:

Input: 
fn = async (a, b) => { 
  await new Promise(res => setTimeout(res, 120)); 
  return a + b; 
}
inputs = [5,10]
t = 150
Output: {"resolved":15,"time":120}
Explanation:
​​​​The function resolved 5 + 10 = 15 at t=120ms. The time limit is never reached.
Example 4:

Input: 
fn = async () => { 
  throw "Error";
}
inputs = []
t = 1000
Output: {"rejected":"Error","time":0}
Explanation:
The function immediately throws an error.
### Intution
The intuition behind this code is to create a time-limited version of a given asynchronous function (fn). The time-limited function should resolve with the result if the original function completes within the specified time limit (t milliseconds). If the execution of the original function exceeds the time limit, the time-limited function should reject with the string "Time Limit Exceeded".

### Approach
The approach taken in the code involves using asynchronous functions, Promises, and the Promise.race method. Here's a breakdown:

The timeLimit function takes two parameters: fn (the original asynchronous function) and t (the time limit in milliseconds).

Inside the timeLimit function, an anonymous asynchronous function (async function (...args)) is created. This function takes any number of arguments (...args), which are then passed to the original function fn.

The original function fn is called with the provided arguments (fn(...args)), and the result is stored in the OriginalPromise.

Simultaneously, a timeoutPromise is created using the setTimeout function. This promise will reject with the message 'Time Limit Exceeded' after the specified time limit (t milliseconds).

The Promise.race method is used to return the first settled promise (either the result of the original function or the timeout). This means that if the original function completes within the time limit, the time-limited function will resolve with the result. If the original function exceeds the time limit, the time-limited function will reject with the message 'Time Limit Exceeded'.
# Complexity
- Time complexity:O(1)


- Space complexity:O(1)


## Code
```javascript
/**
 * @param {Function} fn
 * @param {number} t
 * @return {Function}
 */
var timeLimit = function (fn, t) {

    return async function (...args) {
        const OriginalPromise = fn(...args);
        const timeoutPromise = new Promise((_, rej) => {
            setTimeout(() => {
                rej('Time Limit Exceeded')
            }, t);
        })
        return Promise.race([OriginalPromise, timeoutPromise])
    }
};


```

# 2622. Cache With Time Limit
Write a class that allows getting and setting key-value pairs, however a time until expiration is associated with each key.

The class has three public methods:

set(key, value, duration): accepts an integer key, an integer value, and a duration in milliseconds. Once the duration has elapsed, the key should be inaccessible. The method should return true if the same un-expired key already exists and false otherwise. Both the value and duration should be overwritten if the key already exists.

get(key): if an un-expired key exists, it should return the associated value. Otherwise it should return -1.

count(): returns the count of un-expired keys.
### Intuition
The problem involves implementing a time-limited cache, where each key-value pair has an associated expiration time. The goal is to manage the cache in such a way that when setting a key-value pair, any existing key with the same value gets its expiration reset, and the function returns whether the key was already present. Additionally, the cache should be able to retrieve values by key and provide a count of non-expired keys.

### Approach
The implementation uses a JavaScript class called TimeLimitedCache with three main methods:

set(key, value, duration): Sets a key-value pair with a specified expiration duration. If the key already exists, its expiration is reset, and the function returns true, indicating that the key was already present.
get(key): Retrieves the value associated with a key. If the key is not in the cache or has expired, it returns -1.
count(): Returns the count of non-expired keys in the cache.
The cache uses a Map to store key-value pairs along with their associated expiration timeouts.



### Complexity
- Time complexity:O(1)


- Space complexity:O(n)


## Code
``` javascript
var TimeLimitedCache = function() {
    this.cache=new Map()
};

/** 
 * @param {number} key
 * @param {number} value
 * @param {number} duration time until expiration in ms
 * @return {boolean} if un-expired key already existed
 */
TimeLimitedCache.prototype.set = function(key, value, duration) {
    const alreadyExist=this.cache.get(key)
    if(alreadyExist){
        clearTimeout(alreadyExist.TimeoutId)
    }
    const TimeoutId=setTimeout(()=>{this.cache.delete(key)},duration);
    this.cache.set(key,{value,TimeoutId});
    return Boolean(alreadyExist)
};

/** 
 * @param {number} key
 * @return {number} value associated with key
 */
TimeLimitedCache.prototype.get = function(key) {
    if(this.cache.has(key)){
        return this.cache.get(key).value
    }
    return -1
};

/** 
 * @return {number} count of non-expired keys
 */
TimeLimitedCache.prototype.count = function() {
    return this.cache.size
};

/**
 * const timeLimitedCache = new TimeLimitedCache()
 * timeLimitedCache.set(1, 42, 1000); // false
 * timeLimitedCache.get(1) // 42
 * timeLimitedCache.count() // 1
 */
```
# 2627. Debounce

Given a function fn and a time in milliseconds t, return a debounced version of that function.

A debounced function is a function whose execution is delayed by t milliseconds and whose execution is cancelled if it is called again within that window of time. The debounced function should also receive the passed parameters.

For example, let's say t = 50ms, and the function was called at 30ms, 60ms, and 100ms. The first 2 function calls would be cancelled, and the 3rd function call would be executed at 150ms. If instead t = 35ms, The 1st call would be cancelled, the 2nd would be executed at 95ms, and the 3rd would be executed at 135ms.

The above diagram shows how debounce will transform events. Each rectangle represents 100ms and the debounce time is 400ms. Each color represents a different set of inputs.

Please solve it without using lodash's _.debounce() function.
## Pre-Requisite
### Why to use Debouncing?
Have you ever encountered a situation where a function gets called multiple times within a short amount of time, leading to performance issues or unexpected behavior? This is a common problem in JavaScript, especially when working with events like scrolling, resizing, or typing.

Fortunately, there's a simple technique called "debouncing" that can help you control the frequency of function calls and avoid these issues.

### What is Debouncing?
Debouncing is a method that limits the rate at which a function gets called. It works by delaying the execution of a function until a certain amount of time has passed without any additional function calls. If another function call happens within this time frame, the timer resets and the function execution is delayed again.

Debouncing is useful in situations where you want to prevent a function from being called too frequently, such as:

Handling user input events like keypresses, mouse movements, or button clicks

Handling expensive computations or network requests that don't need to be performed on every function call


## Intuition and Approach
- debounce takes two arguments: fn and t.
- fn is the function that you want to debounce.
- t is the amount of time you want to wait before executing fn after the last time it was called.
- The debounce function returns a new function that takes any number of arguments (...args).
- Within the returned function, a timer is set using setTimeout. The timer is initially set to t milliseconds.
- Every time the returned function is called, the clearTimeout function is called to reset the timer to t milliseconds.
- Once the timer has elapsed without the returned function being called again, the timer's callback function is executed. The callback function calls fn with the arguments that were passed to the returned function.
- The debounce function returns the new function that was created in step 2.
- In simpler terms, the debounce function creates a new function that can only be executed after a certain amount of time has passed without it being called again. 
- This is achieved by creating a timer that is reset every time the debounced function is called. Once the timer has elapsed without the debounced function being called again, the function is executed. This is useful when you want to limit the frequency of some expensive operation, such as making an HTTP request or rendering a large number of elements on a page.


## Complexity
- Time complexity:O(1)


- Space complexity:O(1)

# Code
```javascript
/**
 * @param {Function} fn
 * @param {number} t milliseconds
 * @return {Function}
 */
var debounce = function(fn, t) {
    let timer;
    return function(...args) {
        clearTimeout(timer)
        timer=setTimeout(() => {
      fn.apply(this, args);
    }, t);
    }
};

/**
 * const log = debounce(console.log, 100);
 * log('Hello'); // cancelled
 * log('Hello'); // cancelled
 * log('Hello'); // Logged at t=100ms
 */
```

# 2721. Execute Asynchronous Functions in Parallel
Given an array of asynchronous functions functions, return a new promise promise. Each function in the array accepts no arguments and returns a promise. All the promises should be executed in parallel.

promise resolves:

When all the promises returned from functions were resolved successfully in parallel. The resolved value of promise should be an array of all the resolved values of promises in the same order as they were in the functions. The promise should resolve when all the asynchronous functions in the array have completed execution in parallel.
promise rejects:

When any of the promises returned from functions were rejected. promise should also reject with the reason of the first rejection.
Please solve it without using the built-in Promise.all function.

## Code
```javascript
/**
 * @param {Array<Function>} functions
 * @return {Promise<any>}
 */
var promiseAll = function(functions) {
  return new Promise((resolve, reject) => {
    const results = [];
    let resolvedCount = 0;
    let rejected = false;

    for (let i = 0; i < functions.length; i++) {
      functions[i]()
        .then(result => {
          if (!rejected) {
            results[i] = result;
            resolvedCount++;

            if (resolvedCount === functions.length) {
              resolve(results);
            }
          }
        })
        .catch(error => {
          if (!rejected) {
            rejected = true;
            reject(error);
          }
        });
    }
  });
};

/**
 * const promise = promiseAll([() => new Promise(res => res(42))])
 * promise.then(console.log); // [42]
 */

 ```

 # 2727. Is Object Empty
 Given an object or an array, return if it is empty.

An empty object contains no key-value pairs.
An empty array contains no elements.
You may assume the object or array is the output of JSON.parse.

## Pre-Requisite
The `Object.keys(obj)` method in JavaScript is used to extract the keys (property names) of a given object `obj` and return them as an array. Here's a more detailed explanation:

### Syntax:

```javascript
const keysArray = Object.keys(obj);
```

### Parameters:

- `obj`: The object whose enumerable properties (keys) are to be returned.

### Return Value:

- An array of strings representing the keys of the object.

### Example:

```javascript
const myObject = {
    name: 'John',
    age: 30,
    isStudent: false
};

const keysArray = Object.keys(myObject);
console.log(keysArray);
```

In this example, `keysArray` will be `['name', 'age', 'isStudent']`.

### Notes:

1. **Enumerable Properties:**
   - `Object.keys()` returns only the enumerable properties of the object. Enumerable properties are those properties whose `enumerable` attribute is set to `true`. By default, most built-in properties are enumerable.

2. **Order of Keys:**
   - The order of the keys in the resulting array is not guaranteed to be the same as the order in which they were added to the object. In practice, it often reflects the order in which the properties were added, but this behavior is not standardized.

3. **Non-enumerable Properties:**
   - If an object has non-enumerable properties or properties with the `enumerable` attribute set to `false`, those properties will not be included in the array returned by `Object.keys()`.

### Use Cases:

1. **Checking if an Object is Empty:**
   ```javascript
   if (Object.keys(myObject).length === 0) {
       console.log('The object is empty.');
   } else {
       console.log('The object is not empty.');
   }
   ```

2. **Iterating Over Object Properties:**
   ```javascript
   const keysArray = Object.keys(myObject);
   for (const key of keysArray) {
       console.log(`${key}: ${myObject[key]}`);
   }
   ```

3. **Converting Object to Array of Values:**
   ```javascript
   const valuesArray = Object.keys(myObject).map(key => myObject[key]);
   ```

`Object.keys()` is a versatile method commonly used in JavaScript for various tasks related to working with objects and their properties. It provides a way to access and manipulate the keys of an object in a straightforward manner.
## Code
```javascript
/**
 * @param {Object|Array} obj
 * @return {boolean}
 */
var isEmpty = function(obj) {
    if(Object.keys(obj).length===0){//this method is solution
        return true
    }
    else{
        return false
    }
};
```
# 2677. Chunk Array
Given an array arr and a chunk size size, return a chunked array. A chunked array contains the original elements in arr, but consists of subarrays each of length size. The length of the last subarray may be less than size if arr.length is not evenly divisible by size.

You may assume the array is the output of JSON.parse. In other words, it is valid JSON.

Please solve it without using lodash's _.chunk function.
## Pre-Requisite


### JavaScript Array Methods:

1. **`push(element)`**:
   - Adds one or more elements to the end of an array.

    ```javascript
    let fruits = ['apple', 'banana'];
    fruits.push('orange');
    // fruits is now ['apple', 'banana', 'orange']
    ```

2. **`pop()`**:
   - Removes the last element from an array and returns that element.

    ```javascript
    let fruits = ['apple', 'banana', 'orange'];
    let lastFruit = fruits.pop();
    // lastFruit is 'orange', fruits is now ['apple', 'banana']
    ```

3. **`shift()`**:
   - Removes the first element from an array and returns that element.

    ```javascript
    let fruits = ['apple', 'banana', 'orange'];
    let firstFruit = fruits.shift();
    // firstFruit is 'apple', fruits is now ['banana', 'orange']
    ```

4. **`unshift(element)`**:
   - Adds one or more elements to the beginning of an array.

    ```javascript
    let fruits = ['banana', 'orange'];
    fruits.unshift('apple');
    // fruits is now ['apple', 'banana', 'orange']
    ```

5. **`indexOf(element)`**:
   - Returns the first index at which a given element is found in the array. If the element is not present, it returns -1.

    ```javascript
    let fruits = ['apple', 'banana', 'orange'];
    let index = fruits.indexOf('banana');
    // index is 1
    ```

6. **`slice(start, end)`**:
   - Returns a shallow copy of a portion of an array specified by start and end indices.

    ```javascript
    let fruits = ['apple', 'banana', 'orange', 'kiwi'];
    let subset = fruits.slice(1, 3);
    // subset is ['banana', 'orange']
    ```

7. **`splice(start, deleteCount, item1, item2, ...)`**:
   - Changes the contents of an array by removing or replacing existing elements and/or adding new elements in place.

    ```javascript
    let fruits = ['apple', 'banana', 'orange'];
    fruits.splice(1, 1, 'kiwi');
    // fruits is now ['apple', 'kiwi', 'orange']
    ```

8. **`forEach(callback)`**:
   - Executes a provided function once for each array element.

    ```javascript
    let fruits = ['apple', 'banana', 'orange'];
    fruits.forEach(fruit => console.log(fruit));
    // Outputs:
    // apple
    // banana
    // orange
    ```

9. **`map(callback)`**:
   - Creates a new array with the results of calling a provided function on every element in the array.

    ```javascript
    let numbers = [1, 2, 3];
    let squared = numbers.map(num => num * num);
    // squared is [1, 4, 9]
    ```

10. **`filter(callback)`**:
   - Creates a new array with all elements that pass the test implemented by the provided function.

    ```javascript
    let numbers = [1, 2, 3, 4, 5];
    let evens = numbers.filter(num => num % 2 === 0);
    // evens is [2, 4]
    ```
#### Time Complexity: O(n * size)
#### Space Complexity: O(n)

Code
```javascript

/**
 * @param {Array} arr
 * @param {number} size
 * @return {Array}
 */
var chunk = function(arr, size) {
    // if(arr.length==0){
    //     return [] 
    // }
    const chunkArray=[]
    for(let i=0;i<arr.length;i+=size){
        const chunk=arr.slice(i,i+size)
        chunkArray.push(chunk)
    }
    return chunkArray
};
```
# 2619. Array Prototype Last

Write code that enhances all arrays such that you can call the array.last() method on any array and it will return the last element. If there are no elements in the array, it should return -1.

You may assume the array is the output of JSON.parse.

 

Example 1:

Input: nums = [null, {}, 3]
Output: 3
Explanation: Calling nums.last() should return the last element: 3.
## Code 
```javascript
Array.prototype.last = function(arr) {
   if( this.length===0){
       return -1
   }
   else{
       return this[this.length-1]//myArray[myArray.length - 1];
   }
};
```
# 2724. Sort By

Given an array arr and a function `(fn)`, return a sorted array sortedArr. You can assume fn only returns numbers and those numbers determine the sort order of sortedArr. sortedArray must be sorted in ascending order by `(fn)` output.

You may assume that fn will never duplicate numbers for a given array.


### Approach
Utilize JavaScript's `Array.sort()` method with a custom comparator function. The comparator function calls `fn` on array elements (`a` and `b`) to extract sortable values. The subtraction or comparison result determines the sort order.

### Implementation

#### Implementation 1: Subtraction-Based Comparator

```javascript
function sortBy(arr, fn) {
    arr.sort((a, b) => fn(a) - fn(b));
    return arr;
}
```

#### Implementation 2: Comparison-Based Comparator

```javascript
function sortBy(arr, fn) {
    arr.sort((a, b) => fn(a) < fn(b) ? -1 : 1);
    return arr;
}
```

### Examples

1. Sorting numbers in ascending order:

```javascript
let numbers = [40, 1, 5, 200];
sortBy(numbers, num => num);
// Result: [1, 5, 40, 200]
```

2. Sorting objects based on a specific key:

```javascript
let objects = [{ x: 3 }, { x: 1 }, { x: 2 }];
sortBy(objects, obj => obj.x);
// Result: [{ x: 1 }, { x: 2 }, { x: 3 }]
```

3. Sorting arrays based on the second element:

```javascript
let arrays = [[3, 1], [1, 2], [2, 3]];
sortBy(arrays, arr => arr[1]);
// Result: [[3, 1], [1, 2], [2, 3]]
```

### Complexity

- Time Complexity: O(NlogN)
- Space Complexity: O(N)

### Interview Tips

- Understand the purpose of `Array.sort()` and how a comparator function works.
- Familiarize yourself with sorting objects based on specific properties.
- Consider the use of `String.prototype.localeCompare()` for locale-sensitive sorting.
- Practice sorting numbers in descending order using `Array.sort()`.



# 2722. Join Two Arrays by ID

Given two arrays arr1 and arr2, return a new array joinedArray. All the objects in each of the two inputs arrays will contain an id field that has an integer value. joinedArray is an array formed by merging arr1 and arr2 based on their id key. The length of joinedArray should be the length of unique values of id. The returned array should be sorted in ascending order based on the id key.

If a given id exists in one array but not the other, the single object with that id should be included in the result array without modification.

If two objects share an id, their properties should be merged into a single object:

If a key only exists in one object, that single key-value pair should be included in the object.
If a key is included in both objects, the value in the object from arr2 should override the value from arr1.

Certainly! Below are the implementations for all three approaches along with a short article explaining the importance of merging arrays based on a common key in programming.

## Approach 1: Brute Force

```javascript
function mergeArraysBruteForce(arr1, arr2) {
  // Combine arrays
  const combinedArray = [...arr1, ...arr2];

  // Initialize merged object
  const merged = {};

  // Merge objects based on ID
  combinedArray.forEach(obj => {
    const id = obj.id;
    if (!merged[id]) {
      merged[id] = { ...obj };
    } else {
      merged[id] = { ...merged[id], ...obj };
    }
  });

  // Extract values from merged object
  const joinedArray = Object.values(merged);

  // Sort by ID in ascending order
  joinedArray.sort((a, b) => a.id - b.id);

  return joinedArray;
}
```

## Approach 2: Using Map

```javascript
function mergeArraysUsingMap(arr1, arr2) {
  const map = new Map();

  // Add objects from arr1 to map
  for (const obj of arr1) {
    map.set(obj.id, { ...obj });
  }

  // Merge objects from arr2 based on ID
  for (const obj of arr2) {
    const id = obj.id;
    if (map.has(id)) {
      const existingObj = map.get(id);
      for (const key of Object.keys(obj)) {
        existingObj[key] = obj[key];
      }
    } else {
      map.set(id, { ...obj });
    }
  }

  // Extract values from map and sort by ID
  const joinedArray = [...map.values()].sort((a, b) => a.id - b.id);

  return joinedArray;
}
```

## Approach 3: Using Two Pointers

```javascript
function mergeArraysUsingPointers(arr1, arr2) {
  // Sort arrays in ascending order based on ID
  arr1.sort((a, b) => a.id - b.id);
  arr2.sort((a, b) => a.id - b.id);

  // Initialize pointers and result array
  let i = 0, j = 0;
  const joinedArray = [];

  // Merge based on ID
  while (i < arr1.length && j < arr2.length) {
    if (arr1[i].id < arr2[j].id) {
      joinedArray.push({ ...arr1[i] });
      i++;
    } else if (arr1[i].id > arr2[j].id) {
      joinedArray.push({ ...arr2[j] });
      j++;
    } else {
      // Merge properties if IDs are equal
      joinedArray.push({ ...arr1[i], ...arr2[j] });
      i++;
      j++;
    }
  }

  // Add remaining objects from arr1
  while (i < arr1.length) {
    joinedArray.push({ ...arr1[i] });
    i++;
  }

  // Add remaining objects from arr2
  while (j < arr2.length) {
    joinedArray.push({ ...arr2[j] });
    j++;
  }

  return joinedArray;
}
```

##  Merging Arrays Based on a Common Key in Programming

In various programming scenarios, merging arrays based on a common key proves to be a crucial operation, offering a versatile solution across diverse domains. Whether handling data integration, social media analysis, geographic information systems (GIS), or supply chain management, this approach allows programmers to consolidate information, establish relationships, and enhance data processing.

**Data Integration:**
When dealing with multiple data sources, each providing information in separate arrays with a shared identifier, merging based on this identifier is indispensable. It enables the creation of a unified dataset, facilitating seamless analysis and processing.

**Social Media Analysis:**
In social media analysis, merging arrays based on user or post IDs proves invaluable. This facilitates the comprehensive analysis of user profiles, comments, likes, and shares, empowering analysts to identify popular posts and uncover patterns in user behavior.

**Geographic Information Systems (GIS):**
For GIS applications, merging arrays based on location IDs or geographic identifiers is fundamental. This integration allows for the consolidation of spatial data, combining features, attributes, and relevant information to support spatial analysis and decision-making.

 **Supply Chain Management:**
In supply chain systems, merging arrays based on unique identifiers such as product codes or order IDs is instrumental. It enables the tracking and management of the flow of goods or services, consolidating information from different stages of the supply chain for efficient monitoring and optimization.

### **Approach Variations:**
The presented approaches—brute force, map-based, and two-pointer—are tailored to different scenarios and preferences. The brute force method combines arrays and iteratively merges objects, while the map-based approach efficiently utilizes a data structure. The two-pointer technique, reminiscent of merging sorted arrays, proves effective in scenarios where sorting is acceptable.


# 2625. Flatten Deeply Nested Array

Given a multi-dimensional array arr and a depth n, return a flattened version of that array.

A multi-dimensional array is a recursive data structure that contains integers or other multi-dimensional arrays.

A flattened array is a version of that array with some or all of the sub-arrays removed and replaced with the actual elements in that sub-array. This flattening operation should only be done if the current depth of nesting is less than n. The depth of the elements in the first array are considered to be 0.

Please solve it without the built-in Array.flat method.
### Intuition
The code appears to be a recursive solution to flatten a multi-dimensional array up to a specified depth. It uses a helper function named `helper` to traverse the input array recursively. The base case checks whether an element is an object (array) and if the current depth is less than the specified depth (`n`). If the conditions are met, it continues to flatten the subarray; otherwise, it appends the element to the result.

### Approach
1. The `flat` function initializes an empty array `res` to store the flattened result.
2. The `helper` function is defined inside `flat`, which takes an array (`arr`) and the current depth (`depth`) as parameters.
3. The `helper` function iterates through each element of the array.
   - If the element is an object (array) and the current depth is less than `n`, it calls itself recursively with the subarray and an incremented depth.
   - If the conditions are not met, it appends the element to the `res` array.
4. The `helper` function returns the final flattened result.

### Complexity
- Time complexity: The time complexity of the code is determined by the number of elements in the input array. In the worst case, each element needs to be visited, resulting in a time complexity of O(N), where N is the total number of elements in the array.
- Space complexity: The space complexity is also influenced by the input array. The recursion depth is limited by the depth of the array, and the space required for the result array. In the worst case, the space complexity is O(N), where N is the total number of elements in the array.
## Code
```javascript
/**
 * @param {Array} arr - The input array to be flattened.
 * @param {number} n - The depth up to which the array should be flattened.
 * @return {Array} - The flattened array.
 */
var flat = function (arr, n) {
    // Initialize an empty array to store the flattened result.
    let res = [];

    // Helper function to recursively flatten the array.
    function helper(arr, depth) {
        // Iterate through each element of the array.
        for (const val of arr) {
            // Check if the element is an object (array) and the depth is less than the specified depth (n).
            if (typeof val === 'object' && depth < n) {
                // If conditions are met, recursively call the helper function with the subarray and an incremented depth.
                helper(val, depth + 1);
            } else {
                // If conditions are not met, append the element to the result array.
                res.push(val);
            }
        }
    }

    // Call the helper function with the initial array and depth of 0.
    helper(arr, 0);

    // Return the final flattened result.
    return res;
};

```

# 2705. Compact Object
Given an object or array obj, return a compact object. A compact object is the same as the original object, except with keys containing falsy values removed. This operation applies to the object and any nested objects. Arrays are considered objects where the indices are keys. A value is considered falsy when Boolean(value) returns false.

You may assume the obj is the output of JSON.parse. In other words, it is valid JSON.
## **Intuition and Approach:**

The `compactObject` function is designed to simplify and condense an input object by removing keys with falsy values, handling nested structures as well. Here's an intuitive breakdown:

1. **Base Cases:**
   - If the object is `null`, return `null`.
   - If the object is an array, filter out falsy values and recursively compact each element.
   - If the object is not an iterable (neither an object nor an array), return it as is.

2. **Iterative Case:**
   - If the object is an iterable (object with keys), iterate through its keys.
   - For each key, apply `compactObject` recursively to its corresponding value.
   - If the value is truthy, include the key-value pair in a new compacted object.

3. **Return Result:**
   - Return the final compacted object.

**Time Complexity:**
   - The time complexity is O(n), where n is the number of keys in the input object. The iteration through the keys contributes to the linear time complexity.

**Space Complexity:**
   - The space complexity is O(m), where m is the size of the compacted object. The main factor is the space required for the new compacted object.

 ```javascript       
   // Define a function named compactObject, which takes an object 'obj' as a parameter
var compactObject = function(obj) {
    // Step 1: Check if 'obj' is null
    if (obj === null) return null;

    // Step 2: Check if 'obj' is an array, filter out falsy values, and map each element by calling compactObject recursively
    if (Array.isArray(obj)) return obj.filter(Boolean).map(compactObject);

    // Step 3: Check if 'obj' is not an object, return 'obj' as is
    if (typeof obj !== "object") return obj;

    // If 'obj' is an iterable object, compact its keys and values
    const compacted = {};
    // Step 4: Iterate through each key in the iterable object 'obj'
    for (const key in obj) {
        // Step 5: Recursively call compactObject on the value of the current key
        let value = compactObject(obj[key]);
        // Check if the value is truthy (not null, undefined, or false)
        if (value) {
            // If true, add the key-value pair to the 'compacted' object
            compacted[key] = value;
        }
    }

    // Return the 'compacted' object
    return compacted;
};

```
# 2694. Event Emitter
Design an EventEmitter class. This interface is similar (but with some differences) to the one found in Node.js or the Event Target interface of the DOM. The EventEmitter should allow for subscribing to events and emitting them.

Your EventEmitter class should have the following two methods:

subscribe - This method takes in two arguments: the name of an event as a string and a callback function. This callback function will later be called when the event is emitted.
An event should be able to have multiple listeners for the same event. When emitting an event with multiple callbacks, each should be called in the order in which they were subscribed. An array of results should be returned. You can assume no callbacks passed to subscribe are referentially identical.
The subscribe method should also return an object with an unsubscribe method that enables the user to unsubscribe. When it is called, the callback should be removed from the list of subscriptions and undefined should be returned.
emit - This method takes in two arguments: the name of an event as a string and an optional array of arguments that will be passed to the callback(s). If there are no callbacks subscribed to the given event, return an empty array. Otherwise, return an array of the results of all callback calls in the order they were subscribed.

```javascript
class EventEmitter {
    // Object to store events and their associated callbacks
    eventMap = {};

    // Method to subscribe to an event
    subscribe(event, cb) {
        // If the event doesn't exist in the eventMap, create an empty Set
        if (!this.eventMap.hasOwnProperty(event)) {
            this.eventMap[event] = new Set();
        }

        // Add the callback to the Set of callbacks for this event
        this.eventMap[event].add(cb);

        // Return an object with the unsubscribe method
        return {
            unsubscribe: () => {
                // Remove the callback from the Set when unsubscribed
                this.eventMap[event].delete(cb);
            },
        };
    }

    // Method to emit an event
    emit(event, args = []) {
        let res = [];

        // Iterate over each callback in the Set for the given event
        // If the event doesn't exist, use an empty array
        (this.eventMap[event] ?? []).forEach((cb) => {
            // Call each callback with the provided arguments
            res.push(cb(...args));
        });

        // Return the array of results
        return res;
    }
}
```

### Approach:

1. **subscribe:**
   - A `subscribe` method is implemented to add callbacks to a Set associated with a particular event in the `eventMap`.
   - If the event doesn't exist, a new Set is created for that event.
   - The `subscribe` method returns an object with an `unsubscribe` method, allowing removal of the callback.

2. **emit:**
   - An `emit` method is implemented to trigger callbacks associated with a particular event.
   - It iterates over each callback in the Set for the given event (or an empty array if the event doesn't exist) and calls the callbacks with the provided arguments.
   - The results of the callbacks are stored in an array and returned.

### Complexities:

- **Subscribe:**
  - Time Complexity: O(1) - Adding a callback to a Set has constant time complexity.
  - Space Complexity: O(N) - N is the number of unique events. Each event has its own Set of callbacks.

- **Emit:**
  - Time Complexity: O(K) - K is the number of callbacks for the specific event. It depends on the number of subscribed callbacks for the given event.
  - Space Complexity: O(1) - The space required for the results array.  
  
