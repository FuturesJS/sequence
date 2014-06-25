huzzah for es6-promise!
===

Now that `es6-promise` exists, `Sequence` isn't really necessary. Try `es6-promise` today!

```javascript
var Promise = require('es6-promise').Promise
  , state = {}
  ;
  
new Promise(function (resolve, reject) {
  resolve('a');
}).then(function (a) {
  state.a = a;
  return new Promise(function (resolve, reject) {
    resolve('b');
  });
}).then(function (b) {
  state.b = b;
  console.log(state);
})
```

P.S. Sequence is admittedly a bit more intuitive than and I'd happily accept a pull request that uses es6-promise under the hood.

Sequence
===

Creates an Asynchronous Stack which execute each enqueued method after the previous function calls the provided `next(err, data [, ...])`.

In many cases [`forEachAsync`](https://github.com/FuturesJS/forEachAsync) or [`forAllAsync`](https://github.com/FuturesJS/forAllAsync) will be a better alternative.

API
---

  * `Sequence.create(defaultContext=null)`
  * `then(function callback(next, err, data [, ...]) {}, context)` - add a callback onto the queue
    * begins or resumes the queue
    * passes the results of the previous function into the next

Node.js Installation
---

Node.JS (Server):

```bash
npm install sequence
```

Browser Installation
---

You can install from bower:

```bash
bower install sequence
```

Or download the raw file from <https://raw.github.com/FuturesJS/sequence/master/sequence.js>:

```bash
wget https://raw.github.com/FuturesJS/sequence/master/sequence.js
```

Or build with pakmanager:

```bash
pakmanager build sequence
```

Usage
---

```javascript
(function (exports) {
  'use strict';

  var Sequence = exports.Sequence || require('sequence').Sequence
    , sequence = Sequence.create()
    , err
    ;

  sequence
    .then(function (next) {
      setTimeout(function () {
        next(err, "Hi", "World!");
      }, 120);
    })
    .then(function (next, err, a, b) {
      setTimeout(function () {
        next(err, "Hello", b);
      }, 270);
    })
    .then(function (next, err, a, b) {
      setTimeout(function () {
        console.log(a, b);
        next();
      }, 50);
    });

// so that this example works in browser and node.js
}('undefined' !== typeof exports && exports || new Function('return this')()));
```
