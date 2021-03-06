# node-hledger

Node.js API for [hledger].

```
npm install --save-exact hledger
```

[![Status](https://travis-ci.org/rstacruz/node-hledger.svg?branch=master)](https://travis-ci.org/rstacruz/node-hledger "See test builds")

## API

<!--api-->

### hledger

> `hledger(args, options)`

Invokes hledger and returns a promise. It resolves into the CSV output as a
2D array.

```js
var hledger = require('hledger')

hledger(['bal', 'Assets'])
  .then((data) => ...)

[ [ 'account', 'balance' ],
  [ 'Assets', '$200' ],
  [ 'Assets:Savings', '$150' ],
  [ 'Assets:Checking', '$50' ] ]
```

You can invoke it with a string:

```js
hledger('bal Assets')
```

You can then use functions to make better sense of them:

```js
hledger(['bal', 'Assets'])
  .then(hledger.tableize)
  .then((data) => ...)

[ { account: 'Assets', balance: '$200' },
  { account: 'Assets:Savings', balance: '$150' },
  ... ]
```

You may pass the option `{ mode: 'list' }` to support commands that don't
have CSV output (eg, `accounts`).

```js
hledger('accounts', { mode: 'list' })
.then(data => ...)

[ 'Assets:Savings',
  'Assets:Checking'
]

```

### hledger.tableize

> `tableize(list)`

hledger.tableize:
Turns a CSV-based array into an table list.

```js
input = [
  ['account', 'amount'],
  ['Savings', '$100'],
  ['Checking', '$150']
]

tableize(input)
// [ { account: 'Savings', amount: '$100' },
//   { account: 'Checking', amount: '$200' } ]
```

Used for piping into `hledger()`'s promise output:

```js
hledger('...')
  .then(hledger.tableize)
  .then((data) => ...)
```
<!--api:end-->

[hledger]: http://hledger.org/

## Thanks

**node-hledger** © 2015+, Rico Sta. Cruz. Released under the [MIT] License.<br>
Authored and maintained by Rico Sta. Cruz with help from contributors ([list][contributors]).

> [ricostacruz.com](http://ricostacruz.com) &nbsp;&middot;&nbsp;
> GitHub [@rstacruz](https://github.com/rstacruz) &nbsp;&middot;&nbsp;
> Twitter [@rstacruz](https://twitter.com/rstacruz)

[MIT]: http://mit-license.org/
[contributors]: http://github.com/rstacruz/node-hledger/contributors
