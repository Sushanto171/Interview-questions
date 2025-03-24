# Interview-questions

---

### **1. Closures & Scope**

1. **What is a closure in JavaScript? Provide an example.**  
   A closure is a function that retains access to its outer (lexical) scope even after the outer function has finished executing.  
   Example:
   ```js
   function outer() {
       let x = 10;
       return function inner() {
           return x; // inner retains access to x
       };
   }
   const fn = outer();
   console.log(fn()); // 10
   ```

2. **How does lexical scoping work in JavaScript?**  
   Lexical scoping means a variable’s scope is determined by its location in the source code at the time of definition, not execution. Inner functions have access to variables in their outer scope.  
   Example: In the closure above, `inner` accesses `x` from `outer` because it’s lexically defined there.

3. **What will be the output of the following code?**
   ```js
   function outer() {
       let count = 0;
       return function inner() {
           count++;
           console.log(count);
       };
   }
   const fn = outer();
   fn(); // 1
   fn(); // 2
   ```
   Explanation: `fn` retains access to `count` via closure. Each call increments and logs it: 1, then 2.

4. **What is the difference between `let`, `const`, and `var` in terms of scope and hoisting?**  
   - `var`: Function-scoped, hoisted with `undefined` initialization.  
   - `let`: Block-scoped, hoisted but not initialized (Temporal Dead Zone).  
   - `const`: Block-scoped, hoisted but not initialized, cannot be reassigned (though object properties can mutate).  

5. **How does the JavaScript garbage collector handle closures?**  
   The garbage collector frees memory when objects are no longer referenced. Closures keep outer variables alive as long as the inner function is referenced, potentially delaying garbage collection.

---

### **2. Hoisting & Execution Context**

6. **Explain hoisting in JavaScript. What gets hoisted and in what order?**  
   Hoisting moves variable and function declarations to the top of their scope during compilation.  
   - `var` declarations are hoisted and initialized with `undefined`.  
   - Function declarations are fully hoisted (body included).  
   - `let` and `const` are hoisted but not initialized (TDZ). Order: Variables then functions.

7. **What will be the output of this code?**
   ```js
   console.log(a); // undefined
   var a = 10;
   ```
   Explanation: `var a` is hoisted, initialized as `undefined`, then assigned 10 after the log.

8. **What is the temporal dead zone in JavaScript?**  
   The TDZ is the period between entering a scope and the point where `let` or `const` is initialized. Accessing the variable in this zone throws a `ReferenceError`.

9. **Explain how function declarations and function expressions behave differently in terms of hoisting.**  
   - Function declarations are fully hoisted (name and body).  
   - Function expressions (e.g., `const fn = function() {}`) are not hoisted; only the variable declaration is (if `var`), leading to `undefined` before assignment.

10. **How does JavaScript create an execution context?**  
    Each execution context has:  
    - **Variable Object**: Stores variables/functions.  
    - **Scope Chain**: Links to outer scopes.  
    - **This Binding**: Determines `this` value. Created in two phases: creation (hoisting) and execution.

---

### **3. Prototypes & Object-Oriented JavaScript**

11. **How does prototypal inheritance work in JavaScript?**  
    Objects inherit properties and methods from their prototype. Each object has a `__proto__` link to its prototype, forming a chain that ends at `Object.prototype`.

12. **What is the difference between `__proto__` and `prototype`?**  
    - `__proto__`: An object’s internal reference to its prototype (getter/setter).  
    - `prototype`: A property of constructor functions, used to set the `__proto__` of instances.

13. **Explain the difference between classical and prototypal inheritance.**  
    - Classical: Uses classes (blueprints) with explicit hierarchies (e.g., Java).  
    - Prototypal: Objects inherit directly from other objects via prototypes, more dynamic.

14. **How can you implement inheritance in JavaScript without using ES6 classes?**  
    Use constructor functions and `call`/`prototype`:
    ```js
    function Parent(name) { this.name = name; }
    Parent.prototype.say = function() { return this.name; };
    function Child(name, age) {
        Parent.call(this, name);
        this.age = age;
    }
    Child.prototype = Object.create(Parent.prototype);
    ```

15. **What is the difference between a constructor function and a class in JavaScript?**  
    - Constructor: A regular function with `new` and manual prototype setup.  
    - Class: Syntactic sugar over constructors, with cleaner syntax and built-in inheritance via `extends`.

---

### **4. Asynchronous JavaScript (Event Loop, Promises, Async/Await)**

16. **Explain the event loop in JavaScript.**  
    The event loop manages async operations in the single-threaded JS runtime. It processes the call stack, then microtasks (e.g., Promises), then macrotasks (e.g., `setTimeout`) from the task queue.

17. **What is the difference between microtasks and macrotasks?**  
    - Microtasks (e.g., Promise callbacks): Executed after the current stack, before the next macrotask.  
    - Macrotasks (e.g., `setTimeout`, I/O): Executed in the next event loop iteration.

18. **What will be the output of the following?**
    ```js
    console.log("Start"); // Start
    setTimeout(() => console.log("Timeout"), 0); // Timeout (later)
    Promise.resolve().then(() => console.log("Promise")); // Promise
    console.log("End"); // End
    ```
    Output: `Start`, `End`, `Promise`, `Timeout`.  
    Explanation: Sync code runs first, microtask (`Promise`) next, macrotask (`setTimeout`) last.

19. **What is the difference between `Promise.all()`, `Promise.any()`, `Promise.race()`, and `Promise.allSettled()`?**  
    - `Promise.all()`: Resolves when all promises resolve, rejects on first rejection.  
    - `Promise.any()`: Resolves with the first fulfilled promise, rejects if all fail.  
    - `Promise.race()`: Resolves/rejects with the first settled promise.  
    - `Promise.allSettled()`: Resolves with the status of all promises (fulfilled or rejected).

20. **How does async/await work under the hood?**  
    `async` functions return a Promise. `await` pauses execution, queuing the rest as a microtask, handled by the event loop when the awaited Promise resolves.

---

### **5. ES6+ Features**

21. **What are the differences between `var`, `let`, and `const`?**  
    Answered in Q4: `var` (function-scoped, hoisted), `let` (block-scoped, TDZ), `const` (block-scoped, immutable binding).

22. **How does destructuring work in JavaScript?**  
    Extracts values from arrays/objects into variables:  
    ```js
    const [a, b] = [1, 2]; // a=1, b=2
    const { x, y } = { x: 3, y: 4 }; // x=3, y=4
    ```

23. **What is the purpose of optional chaining (`?.`) in JavaScript?**  
    Safely accesses nested properties without throwing errors if a reference is `null`/`undefined`:  
    ```js
    const obj = { a: { b: 1 } };
    console.log(obj?.a?.b); // 1
    console.log(obj?.c?.d); // undefined
    ```

24. **Explain the difference between the spread (`...`) and rest (`...`) operators.**  
    - Spread: Expands elements (e.g., `[...arr]` copies an array).  
    - Rest: Collects remaining elements into an array (e.g., `function(a, ...rest)`).

25. **How do default parameters work in JavaScript functions?**  
    Provides fallback values if arguments are `undefined`:  
    ```js
    function greet(name = "Guest") { return `Hi, ${name}`; }
    greet(); // "Hi, Guest"
    ```

---

### **6. Functional Programming in JavaScript**

26. **What is the difference between `map()`, `forEach()`, `filter()`, and `reduce()`?**  
    - `map()`: Transforms each element, returns a new array.  
    - `forEach()`: Executes a function per element, no return.  
    - `filter()`: Returns a new array with elements passing a test.  
    - `reduce()`: Accumulates elements into a single value.

27. **What is a higher-order function? Give an example.**  
    A function that takes or returns a function:  
    ```js
    const add = x => y => x + y;
    const add5 = add(5);
    console.log(add5(3)); // 8
    ```

28. **Explain the concept of currying in JavaScript.**  
    Currying transforms a function with multiple arguments into a sequence of single-argument functions:  
    ```js
    const curry = (a) => (b) => a + b;
    curry(2)(3); // 5
    ```

29. **What is the difference between a pure function and an impure function?**  
    - Pure: Same input always gives same output, no side effects.  
    - Impure: May depend on external state or cause side effects (e.g., modifying global variables).

30. **What is memoization, and how can it be implemented in JavaScript?**  
    Memoization caches function results for reuse:  
    ```js
    function memoize(fn) {
        const cache = {};
        return (...args) => cache[args] || (cache[args] = fn(...args));
    }
    ```

---

### **7. Memory Management & Performance**

31. **How does JavaScript handle memory management?**  
    Uses a garbage collector (e.g., Mark-and-Sweep) to reclaim memory from unreferenced objects.

32. **What is a memory leak, and how can you prevent it?**  
    A memory leak occurs when unused objects remain referenced. Prevent by:  
    - Removing event listeners.  
    - Nullifying references when done.

33. **What are weak references in JavaScript, and how does `WeakMap` help in garbage collection?**  
    Weak references don’t prevent garbage collection. `WeakMap` holds keys weakly, allowing cleanup if no other references exist.

34. **How do closures impact memory usage in JavaScript?**  
    Closures retain outer scope variables, preventing garbage collection until the closure is dereferenced.

35. **What are some techniques to optimize JavaScript performance?**  
    - Minimize DOM access.  
    - Debounce/throttle event handlers.  
    - Use `requestAnimationFrame` for animations.  
    - Avoid memory leaks.

---

### **8. Advanced JavaScript Concepts**

36. **What are generators in JavaScript, and how do they work?**  
    Generators are functions that pause and resume execution, yielding values:  
    ```js
    function* gen() {
        yield 1;
        yield 2;
    }
    const it = gen();
    console.log(it.next().value); // 1
    ```

37. **What is the difference between synchronous and asynchronous iterations?**  
    - Sync: Iterates sequentially (e.g., `for...of`).  
    - Async: Handles promises (e.g., `for await...of`).

38. **Explain how `Symbol` and `BigInt` work in JavaScript.**  
    - `Symbol`: Unique, immutable identifiers (e.g., `Symbol('id')`).  
    - `BigInt`: Arbitrary-precision integers (e.g., `123n`).

39. **How does JavaScript handle floating-point precision errors?**  
    Uses IEEE 754 standard, causing issues like `0.1 + 0.2 !== 0.3`. Fix with rounding or libraries.

40. **What is the difference between deep copy and shallow copy in JavaScript?**  
    - Shallow: Copies top-level properties, references nested objects.  
    - Deep: Recursively copies all levels (e.g., `JSON.parse(JSON.stringify(obj))`).

---

### **9. Web APIs & DOM Manipulation**

41. **How does `localStorage`, `sessionStorage`, and `cookies` differ?**  
    - `localStorage`: Persistent, per-origin storage.  
    - `sessionStorage`: Session-only, per-tab storage.  
    - `cookies`: Small, sent with requests, expiration options.

42. **What is the difference between `innerHTML`, `textContent`, and `innerText`?**  
    - `innerHTML`: Gets/sets HTML content.  
    - `textContent`: Gets/sets raw text, ignores tags.  
    - `innerText`: Gets visible text, considers CSS.

43. **Explain the difference between event delegation and event bubbling.**  
    - Bubbling: Events propagate up the DOM tree.  
    - Delegation: Handle events on a parent, using bubbling to target children.

44. **How does `requestAnimationFrame()` work compared to `setTimeout()`?**  
    - `requestAnimationFrame`: Syncs with browser repaint, smoother animations.  
    - `setTimeout`: Fixed delay, less precise.

45. **What is the difference between `MutationObserver` and `IntersectionObserver`?**  
    - `MutationObserver`: Watches DOM changes.  
    - `IntersectionObserver`: Detects element visibility in viewport.

---

### **10. Security & Best Practices**

46. **What are common security vulnerabilities in JavaScript (e.g., XSS, CSRF)?**  
    - XSS: Injecting malicious scripts (sanitize inputs).  
    - CSRF: Unauthorized requests (use tokens).

47. **How can you prevent prototype pollution in JavaScript?**  
    Avoid modifying `Object.prototype`; use `Object.create(null)` or freeze objects.

48. **What are the best practices for writing secure JavaScript code?**  
    - Validate/sanitize inputs.  
    - Use strict mode.  
    - Avoid `eval()`.  
    - Implement CSP.

49. **What is Content Security Policy (CSP), and how does it help prevent attacks?**  
    CSP restricts resource sources (e.g., scripts), mitigating XSS by whitelisting origins.

50. **How does JavaScript handle same-origin policy and CORS?**  
    - Same-origin: Restricts cross-origin access.  
    - CORS: Allows controlled cross-origin requests via headers (e.g., `Access-Control-Allow-Origin`).

---
