# #JS-FP Intro

Erik Sutherland

> https://github.com/MrRacoon/js-fp-intro

----

## About Me

* FP Enthusiast
* BS CS/Psych
* Database Researcher
* Started in DevOps (~ 4 yrs)
* Landed in Frontend (~ 3 yrs)
* Affinity for Haskell

----

## Main themes in this talk

* Type Signatures
* Composition
* Chaining
* Currying

---

## Funtional Programming

> Functional programming  is the process of building software by composing pure functions, avoiding shared state, mutable data, and side-effects. Functional programming is declarative rather than imperative, and application state flows through pure functions.

> Eric Elliot in [Master the JavaScript Interview: What is Functional Programming?](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-functional-programming-7f218c68b3a0)

---

## FP in JS

* First class functions
* Partial application (`Function.bind()`)
* Mostly enabled through libraries (`lodash`, `ramda`)

----

## Main themes of [Lodash](https://lodash.com)

* Advocates chaining
* Mutable functions
* Lazy evaluation

----

## Main themes of [Ramda](http://ramdajs.com)

* Advocates Composition
* Fixed arity and curried functions
* Arguments are in reverse order from lodash
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

----

## Types Signatures

```javascript
// add :: Number -> Number -> Number
add = (a, b) => a + b;

// map :: (a -> b) -> [a] -> [b]
map = (fn, list) => list.map(fn);
```

----

## Composition

#### It's the law

```javascript
// f :: a -> b
// g :: b -> c
// h :: c -> d

// compose :: ...fns -> (...args -> a)
compose(h, g, f)(x) === h(g(f(x)));

// pipe :: ...fns -> (...args -> a)
pipe(f, g, h)(x) === h(g(f(x)));
```

----

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

## The data

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

## Native Example

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

----

## Native Example

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

```javascript
// vegieNames :: [Produce] -> [String]
const vegieNames = list => R.compose(
  getNames,
  onlyVegetables,
)(list);

vegieNames(data); // ['cauliflower']
```

----

## Composition Example

#### Or use pipe

```javascript
// vegieNames :: [Produce] -> [String]
const vegieNames = list => R.pipe(
  onlyVegetables,
  getNames,
)(list);

vegieNames(data); // ['cauliflower']
```

----

## Composition Example

#### Remove the redundancy

```javascript
// vegieNames :: [Produce] -> [String]
const vegieNames = R.pipe(
  onlyVegetables,
  getNames,
);

vegieNames(data); // ['cauliflower']
```

---

## Lodash Example

#### The Functions

```javascript
// onlyVegetables :: [Produce] -> [Produce]
const onlyVegetables = list => _.filter(
  list,
  obj => obj.type === 'vegetable',
);

// getNames :: [Produce] -> [String]
const getNames = list => _.map(
  list,
  obj => obj.name,
);
```

----

## Lodash Example

#### Compose them?

```javascript
// vegieNames :: [Produce] -> [String]
const vegieNames = list => _.chain(list)
  .filter(obj => obj.type === 'vegetable')
  .map(obj => obj.name)
  .value();
```

----

## Lodash Example

#### Compose with reuse

```javascript
// vegieNames :: [Produce] -> [String]
const vegieNames = list => _.chain(list)
  .thru(onlyVegetables)
  .thru(getNames)
  .value();
```

----

## Lodash Example

#### In place

```javascript
const vegieNames = list =>
  _.map(
    _.filter(
      list,
      obj => obj.type === 'vegetable'
    ),
    obj => obj.name
  );
```

---

## Ramda Example

#### The Functions

```javascript
// onlyVegetables :: [Produce] -> [Produce]
const onlyVegetables = list => R.filter(
  obj => obj.type === 'vegetable',
  list,
);

// getNames :: [Produce] -> [String]
const getNames = list => R.map(
  obj => obj.name,
  list,
);
```

----

## Ramda Example

#### Add the curry

```javascript
// onlyVegetables :: [Produce] -> [Produce]
const onlyVegetables = R.filter(
  obj => obj.type === 'vegetable',
);

// getNames :: [Produce] -> [String]
const getNames = R.map(
  obj => obj.name,
);
```

----

## Ramda Example

#### replace the innards

```javascript
// onlyVegetables :: [Produce] -> [Produce]
const onlyVegetables = R.filter(
  R.whereEq({ type: 'vegetables' })
);

// getNames :: [Produce] -> [String]
const getNames = R.map(
  R.prop('name')
);
```

----

## Ramda Example

#### Compose them

```javascript
// vegieNames :: [Produce] -> [String]
const vegieNames = pipe(
  onlyVegetables,
  getNames,
);
```

----

## Ramda Example

#### Cut out the middleman

```javascript
// vegieNames :: [Produce] -> [String]
const vegieNames = pipe(
  R.filter(R.whereEq({ type: 'vegetables' })),
  R.map(R.prop('name'))
);
```

---

## Why is FP a good thing

* Facilitates good practices
* Puts names to common patterns
* Write less code
* Read less code
* Immutability
* Name less variables
* Can prove a function's correctness (to an extent)

----

## Swiss army effect

* All functions serve a purpose
* Each encodes a **pattern**
* Recognizing the input and the output types
* Using the shortest path to go from your input to output
* Collecting the knowledge of each function, and it's purpose
* Golf

----

## Tips

* Read the docs
* Collect all the patterns

---

## Questions

---

## Contact

* Name: `Erik Sutherland`
* Email: `erik(dot)sutherland(at)nike(dot)com`
* Slack: `@racoon`
* Github: `MrRacoon`
