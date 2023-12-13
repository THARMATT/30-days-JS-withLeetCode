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