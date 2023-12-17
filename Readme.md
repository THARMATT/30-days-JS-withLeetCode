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


