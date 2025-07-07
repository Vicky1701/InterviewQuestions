# JavaScript Interview Questions

## Core JavaScript Concepts

1. Explain event delegation in JavaScript
2. What is hoisting? How does it work with var, let, and const?
3. Difference between == and ===
4. What is closure? Give a practical example.
5. Explain the this keyword and its behavior in different contexts (global, function, arrow, object method).
6. What are promises? How do they differ from callbacks?
7. Explain async/await and error handling with try-catch.
8. What is the Temporal Dead Zone (TDZ)?
9. Explain shallow copy vs. deep copy in JavaScript.
10. How does the JavaScript event loop work? (Microtasks vs. Macrotasks)

## ES6+ Features

11. Difference between let, const, and var.
12. What are template literals?
13. Explain arrow functions vs. regular functions.
14. What are destructuring and spread/rest operators?
15. What are default parameters?
16. Explain Map vs. Object and Set vs. Array.
17. What are generators in JavaScript?
18. Explain Proxy and Reflect in JavaScript.

## DOM & Browser APIs

19. How does event bubbling and capturing work?
20. Difference between event.preventDefault() and event.stopPropagation().
21. What is the difference between document.getElementById() and document.querySelector()?
22. How would you dynamically add/remove a CSS class?
23. Explain localStorage, sessionStorage, and cookies.
24. What is the difference between window.onload and DOMContentLoaded?

## Asynchronous JavaScript

25. Explain setTimeout vs. setInterval.
26. What is the difference between Promise.all, Promise.race, and Promise.allSettled?
27. How would you handle multiple API calls in parallel?
28. What is a race condition in JavaScript? How can you prevent it?
29. Explain Web Workers and their use cases.

## JavaScript Tricky Questions

30. What will be the output?
```javascript
console.log(1 + "2" + "2");  
console.log(1 + +"2" + "2");  
console.log("A" - "B" + "2");  
```

31. What is the output of the following closure example?
```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}
```
How would you fix it to print 0, 1, 2?

32. Explain NaN and how to check if a value is NaN.
33. What is the difference between null, undefined, and undeclared?
34. Explain Function.prototype.bind with an example.
