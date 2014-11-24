# Execon

Patterns of function calls.

## Install

```
npm i execon --save
```

## Api

```js
    var exec = require('execon');
```

### exec
Check is parameter is function, if it's - executes it with given parameters

Was:

```js
function(callback, p1, p2, pN) {
    if (typeof callback === 'function')
        callback(p1, p2, pN);
}
```

Now

```js
function(callback, p1, p2, pN) {
    exec(callback, p1, p2, pN);
}
```

or just

```js
exec.ret(callback, p1, p2, pN);
```

### exec.if
Conditional execution one of two functions

Preconditions:

```js
function one() {
    console.log(1);
}

function two(callback) {
    setTimeout(callback, 1000);
}
```


Before:

```js
if (2 > 3)
    one();
else
    two(one);
    
```

After:

```js
exec.if(2 > 3, one, two);
```

### exec.parallel
if a you need a couple async operation do same work, and then call callback, this function for you.

**Node.js example**.

```js
var fs      = require('fs'),
    Util    = require('util-io');

exec.parallel([
    function(callback) {
        fs.readFile('file1', callback);
    },
    function(callback) {
        fs.readFile('file2',  callback);
    }
], function(error, data1, data2) {
    if (error)
        console.log(error)
    else
        console.log(data1, data2);
});
```
**Vanilla js example.**

```js
var ONE_SECOND  = 1000,
    TWO_SECONDS = 2000,
    func        = function(time, str, callback) {
        setTimeout(function() {
            console.log(str);
            callback(null, str);
        }, time);
    },
    
    func1       = func.bind(null, TWO_SECONDS, 'first'),
    func2       = func.bind(null, ONE_SECOND, 'second');

exec.parallel([func1, func2], function(error, str1, str2) {
    console.log(str1, str2);
});
```

### exec.series
executes functions one-by-one

```js
function one(callback){
    setTimeout(function() {
        console.log(1)
        callback();
    }, 1000);
}

function two(callback) {
    console.log(2);
    callback();
}

exec.series([one, two]);
```

## License

MIT
