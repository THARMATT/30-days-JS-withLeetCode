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
```
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
```
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
