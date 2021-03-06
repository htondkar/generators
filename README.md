# Generators
Snippets about generators
---

General notes: 
When using a generator as an observer, it is important to note that the only purpose of the first invocation of next() is to start the observer. Therefore, you can’t send input via the first next() – you even get an error if you do

yield binds very loosely, so that we don’t have to put its operand in parentheses:
`yield a + b + c` is the same as `yield (a + b + c)`

---

how to use generators:

```
function* getNumbers() {
  yield 1
  yield 5
  yield 10
}
```

Usage: 

```
// looping with for-of syntax
for (let i of getNumbers()) {
  console.log(i)
}

// destructering
let [a, b, c] = getNumbers()


// spread operator
let spreaded = [...getNumbers()]

// Ramda reduce for example
const reducing = reduce((xs, x) => [...xs, x], [], getNumbers())
```

### Coroutine
will run a generator to compeletion, the generator can yield Promises or other normal data
```
function coroutine(generator) {
  const iterator = generator()
  
  return new Promise((resolve, reject) => {
    const run = result => {
      const { value, done } = iterator.next(result)
      
      if (done) return resolve(result)
      
      value
        .then(res => run(res))
        .catch(err => iterator.throw(err))
    }
    
    run()
  })
}
```

example usage:
```
const get = coroutine(function* getData() {
  const otherData = yield fetchOtherData()
  console.log('fetched other data: ', otherData)
  
  const users = yield fetchUsers(otherData)
  console.log('fetched users: ', users)
  
  return users
}).then(data => console.log(data))
```


---

iterate over objects in a for-of loop

```
Object.prototype[Symbol.iterator] = function* () {
  const propKeys = Reflect.ownKeys(this);
  for (const propKey of propKeys) {
      yield [propKey, this[propKey]];
  }
}
```

Usage:

```
for (const [key, value] of {a: 1, b: 2}) {
  console.log(key, value) // a,1 b,2
}

[...{a: 1, b: 2}]

const [[key1, value1], ...] = {a: 1, ...}
```

---

The yield* operator lets you call another generator from within a generator.
yield* considers end-of-iteration values:

```
function* genFuncWithReturn() {
    yield 'a';
    yield 'b';
    return 'The result';
}

function* logReturned(genObj) {
    let result = yield* genObj;
    console.log(result); // 'The result'
}
```

---
