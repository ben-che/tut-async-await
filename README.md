# tut-async-await
Before we begin, I think it's always good to have some sort of background context to how asynchrony in Javascript works. Javascript is a single thread language - this pretty much means that Javascript can only do one thing at a time (things change a bit in the context of the browser and with the introduction of [Web Workers](https://en.wikipedia.org/wiki/Web_worker), but that's another story). Because Javascript is used to make HTTP requests, asynchronous execution had to be introduced. This means that while we wait for a request to be served, other lines of code can still be executed.

This, however, posed a problem: 

```
let printMe = (str) => {
  setTimeout(
    () => {
      console.log(string)
    }, 
    Math.floor(Math.random() * 100) + 1
  )
}

let printAll = () => {
    printMe("a");
    printMe("b");
    printMe("c");
}

printAll()
```

Because of the async nature of the `setTimeout()` function, the order in which the letters will be randomized.

## Javascript Callbacks
A callback is a function that's passed to another function. We can fix the problem above with a callback:

```
let printMe = (str, callbackFn) => {
  setTimeout(
    () => {
      console.log(str);
      callbackFn()
    }, 
    Math.floor(Math.random() * 100) + 1
  )
}

let printAll = () => {
    printMe("a", () => {
        printMe("b", () => {
            printMe("c", () => {
                console.log('done printing')
            })
        })
    })
}

printAll();

```

This way, each function will execute in order, no matter what the value of the `setTimeout()` timer evalutes to. However, this doesn't make for neat code - imagine what happens in bigger files when you have to nest callbacks many times. It gets fairly confusing, and that's what Promises in Javascript try to address.

## Javascript Promises
[Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) allow us to eliminate the use of callbacks up to a degree, and allows for much cleaner code. If we were to solve the same problem above, but with promises:

```
let printMe = (str) => {
    return new Promise((resolve, reject) => {
        setTimeout(
            () => {
            console.log(str);
            resolve();
            }, 
            Math.floor(Math.random() * 100) + 1
        )
    })
};

let printAll = () => {
    printMe("a")
    .then(() => {
        printMe("b")
    })
    .then(() => {
        printMe("c")
    })
    .catch((error) => {
        console.log(error);
    })
};

printAll();
```
This looks a lot cleaner than before, but we can achieve the same results with async/await. 

## Async / Await
At its core, async await are Promises with the addition of syntatic sugar. We can scrap the nesting from callbacks and the use of brackets in promises, and we will be left with code that looks almost synchronous in nature.

```
// this is the same function definition as the promise section

let printMe = (str) => {
    return new Promise((resolve, reject) => {
        setTimeout(
            () => {
            console.log(str);
            resolve();
            }, 
            Math.floor(Math.random() * 100) + 1
        )
    })
};

let printAll = async () => {
  await printMe("a");
  await printMe("b");
  await printMe("c");
}

printAll()
```