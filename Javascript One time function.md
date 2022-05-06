## How to create a javascript function that runs only once?
We'll try to create a logger utility that can be initiated only once in the code.

#### Approach-1: Closures

```javascript
let logger = null;

function init() {
    let called = false;
    return function() {
        if(!called) {
            called = true;
            logger = (type, msg) => {
                console.log(type, msg);
            }
        }
    }
}

const initLogger = init();
initLogger(); // initializes the logger
initLogger(); // This won't re-assign logger method. Silently Fails

logger('error', 'An error occurred!'); // error An error occurred!
```

#### Approach-2: use `delete` operator

```javascript
let logger = null;

const loggerInterface = {
    init() {
        logger = (type, msg) => {
            console.log(type, msg);
        }
        delete loggerInterface.init;
    },
}

loggerInterface.init(); // initializes the logger
loggerInterface.init(); // This will throw error as the key 'init' is deleted

logger('info', 'Task completed!'); // info Task completed!
```