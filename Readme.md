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

### Why to use Debouncing?
Have you ever encountered a situation where a function gets called multiple times within a short amount of time, leading to performance issues or unexpected behavior? This is a common problem in JavaScript, especially when working with events like scrolling, resizing, or typing.

Fortunately, there's a simple technique called "debouncing" that can help you control the frequency of function calls and avoid these issues.

### What is Debouncing?
Debouncing is a method that limits the rate at which a function gets called. It works by delaying the execution of a function until a certain amount of time has passed without any additional function calls. If another function call happens within this time frame, the timer resets and the function execution is delayed again.

Debouncing is useful in situations where you want to prevent a function from being called too frequently, such as:

Handling user input events like keypresses, mouse movements, or button clicks

Handling expensive computations or network requests that don't need to be performed on every function call


## Intuition and Approach
debounce takes two arguments: fn and t.
fn is the function that you want to debounce.
t is the amount of time you want to wait before executing fn after the last time it was called.
The debounce function returns a new function that takes any number of arguments (...args).
Within the returned function, a timer is set using setTimeout. The timer is initially set to t milliseconds.
Every time the returned function is called, the clearTimeout function is called to reset the timer to t milliseconds.
Once the timer has elapsed without the returned function being called again, the timer's callback function is executed. The callback function calls fn with the arguments that were passed to the returned function.
The debounce function returns the new function that was created in step 2.
In simpler terms, the debounce function creates a new function that can only be executed after a certain amount of time has passed without it being called again. This is achieved by creating a timer that is reset every time the debounced function is called. Once the timer has elapsed without the debounced function being called again, the function is executed. This is useful when you want to limit the frequency of some expensive operation, such as making an HTTP request or rendering a large number of elements on a page.


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