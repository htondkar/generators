# generators
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
for (let i of getNumbers()) {
  console.log(i) // 1 5 10
}

// destructering
let [a, b, c] = getNumbers()


// spread operator
let spreaded = [...getNumbers()]

// Ramda reduce for example
const reducing = reduce((xs, x) => [...xs, x], [], getNumbers())
```

