# tut-async-await
Before we begin, I think it's always good to have some sort of background context to how asynchrony in Javascript works. Javascript is a single thread language - this pretty much means that Javascript can only do one thing at a time (things change a bit in the context of the browser and with the introduction of [Web Workers](https://en.wikipedia.org/wiki/Web_worker), but that's another story). Because Javascript is used to make HTTP requests, asynchronous execution had to be introduced. This means that while we wait for a request to be served, other lines of code can still be executed.

This, however, posed a problem: 

```
console.log("a");

setTimeout( () => {
    console.log("b")
    }, 500);

console.log("c");
```

Because of the async nature of the `setTimeout()` function, the order in which the letters are printed will always be "a", "c" then "b".

## Javascript Callbacks
