# Generators
Snippets about generators
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
