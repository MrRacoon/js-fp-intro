# #JS-FP Intro

Erik Sutherland

> https://github.com/MrRacoon/js-fp-intro

---

## About Me

* FP Enthusiast
* BS CS/Psych from PSU
* Affinity for Haskell
* Started in DevOps (wootz 4 linux)
* Frontend for 5 years

---

## Funtional Programming

> Functional programming  is the process of building software by composing pure functions, avoiding shared state, mutable data, and side-effects. Functional programming is declarative rather than imperative, and application state flows through pure functions.

> Eric Elliot in [Master the JavaScript Interview: What is Functional Programming?](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-functional-programming-7f218c68b3a0)

---

## FP in JS

* First class functions
* Partial application (`.bind`)
* Mostly enabled through libraries (`lodash`, `ramda`)

---

## Main themes of Lodash

* Advocates chaining
* Employs cool tricks for lazy evaluation

---

## Main themes of Ramda

* Advocates Composition
* All functions are fixed arity and curried
* Arguments are in reverse order from lodash (for composition)
* Partial lazy evaluation support
* Functions are immutable

---

## Types

* Boolean
* String
* Number
* [a] (lists)
* {String: v} (objects/maps)
* a, b, \* (generic)
* (a -> b) (unary function)
* (a -> b -> c) (binary function)
* (*... -> c) (varadic argument function)

---

## Types examples

```javascript
// add :: Number -> Number -> Number
add = (a, b) => a + b;

// map :: (a -> b) -> [a] -> [b]
map = (fn, list) => list.map(fn);
```

---

## Composition

#### It's the law

```
f :: a -> b
g :: b -> c
h :: c -> d

// compose :: ...fns -> (...args -> a)

compose(h, g, f)(x) === h(g(f(x)));
pipe(f, g, h)(x) === h(g(f(x)));
```

---

## Currying

```javascript
// add :: Number -> Number -> Number
const add = curry((a, b) => a + b);

// add10 :: Number -> Number
const add10 = add(10);

// result :: Number
const result = add10(2); // 12
```

---

## Composition Example

#### Some data

```javascript
// data :: [Produce]
const data = [
  {
    name: 'oranges',
    type: 'fruit',
  },
  {
    name: 'cauliflower',
    type: 'vegetable',
  },
  {
    name: 'apples',
    type: 'fruit',
  },
];

```

---

## Composition Example

#### Some functions

```javascript
// onlyVegetables :: [Produce] -> [Produce]
const onlyVegetables = list =>
  list.filter(
    obj => obj.type === 'vegetable',
  );

// getNames :: [Produce] -> [String]
const getNames = list =>
  list.map(
    obj => obj.name,
  );
```

---

## Composition Example

#### Calling them together

```javascript
// vegieNames :: [Produce] -> [String]
const vegieNames = list =>
  getNames(onlyVegetables(list));

vegieNames(data); // ['cauliflower']
```

---

## Composition Example

#### Add in some compose

```
const vegieNames = list => compose(
  getNames,
  onlyVegetables,
)(list);

vegieNames(data); // ['cauliflower']
```

---

## Composition Example

#### Or use pipe

```
const vegieNames = list => pipe(
  onlyVegetables,
  getNames,
)(list);

vegieNames(data); // ['cauliflower']
```

---

## Composition Example

#### Sprinkle in some Curry

```
const vegieNames = pipe(
  onlyVegetables,
  getNames,
);

vegieNames(data); // ['cauliflower']
```

---

## Composition Example

#### Revisiting the functions

```javascript
// onlyVegetables :: [Produce] -> [Produce]
const onlyVegetables = filter(
  obj => obj.type === 'vegetable',
);

// getNames :: [Produce] -> [String]
const getNames = map(
  obj => obj.name,
);
```

---

## Composition Example

#### The extra mile

```javascript
// onlyVegetables :: [Produce] -> [Produce]
const onlyVegetables = filter(
  whereEq({ type: 'vegetables' })
);

// getNames :: [Produce] -> [String]
const getNames = map(
  prop('name')
);
```

---

## Composition Example

#### Cut out the middleman

```
// before
const getVegies = pipe(
  onlyVegetables,
  getNames,
);

// after
const getVegies = pipe(
  filter(whereEq({ type: 'vegetables' })),
  map(prop('name'))
);
```

---

## Swiss army effect

* All functions serve a purpose
* each one encodes a **pattern**
* Recognizing the input and the output types
* Using the shortest path to go from your input to output
* Collecting the knowledge of each function, and it's purpose
* then, Golf

---

## Why is FP a good thing

* Facilitates good practices (arrays of one type)
* Puts names to common patterns
* Write less code
* Read less code
* Immutability
* No need to name things
* Can prove a function's correctness (to an extent)

---

## Tips

* Read the docs
* Collect all the patterns

---

## Questions

---

## Contact

* Name: `Erik Sutherland`
* Email: `erik.Sutherland@nike.com`
* Slack: `@racoon`
* Github: `MrRacoon`
