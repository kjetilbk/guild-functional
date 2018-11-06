# Why Functional Programming?

---

# Why Functional Programming?

Intention:

Basic introduction to FP, to begin motivating its use and also mention topics we can cover later.

---

## Functional Programming's bold claims:

- Easier to refactor
- Easy to "reason about" the code
- Concurrency made high-level and easy
- Less bugs?

---

## Okay, how?

Several "levels" of how:

- Functional principles
- Functional language features in mainly imperative languages
- Functional or semi-functional languages

Will come back to these.

---

## So what is FP?

In its purest, FP is about programming with functions, where functions are:

1. Total. For every input, they return an output.
2. Deterministic. For the same input, they always return the same output.
3. Pure. Their only effect is computing the return value.

---

## Total

---

## Total

> For every input, total functions return an output.

Example: the `int()` function in Python is not total (`parseInt` in js, `Integer.parseInt` in java):

```python
int("2") # 2
int("abc") # Crash! Not total

```

---

## Total

> For every input, total functions return an output.

Example: the `+` function *is* total.

C++:

```c++
int add(int firstAddend, int secondAddend)
{
    return firstAddend + secondAddend;
}

```


---

## Total

> For every input, total functions return an output.

Benefit:
If all functions were total, you could always trust
the value returned – it will not crash, it won't return `null`.

Sadly:
Not possible in all languages. If your HTTP request fails in javascript or python, what do you do?

---

## Deterministic

---

## Deterministic

> For the same input, deterministic functions always return the same output.

Python:

```python
someState = ""

def nonDeterministicGreeting(name):
  state = someState
  someState = someState + " again"
  return "Hello" + state + " " + name

nonDeterministicGreeting("functional guild") # -> "Hello functional guild"
nonDeterministicGreeting("functional guild") # -> "Hello again functional guild"
nonDeterministicGreeting("functional guild") # -> "Hello again again functional guild"
```

---

## Deterministic

> For the same input, deterministic functions always return the same output.

Python:

```python
def deterministicGreeting(name):
  return "Hello, " + name

deterministicGreeting("functional guild") # -> "Hello functional guild"
deterministicGreeting("functional guild") # -> "Hello functional guild"
...
...
...
deterministicGreeting("functional guild") # -> "Hello functional guild"
```

---

## Deterministic

> For the same input, deterministic functions always return the same output.

Benefits:

- You can trust the function to only depend on what you pass it
- Super easy to test without need for mocking the "whole world", i.e. the global state. For testing, just pass the correct parameters!

---

## Pure

---

## Pure

> Pure functions' only effect is computing the return value.

Typical pure functions:

- Plus / Minus
- Multiply
- toUpperCase
- max()

---

## Pure

> Pure functions' only effect is computing the return value.

Typical not pure functions (in imperative languages):

- Functions that execute HTTP requests, communicate with user, mutate state outside function

---

## Pure

> Pure functions' only effect is computing the return value.

```python
myFavoriteState = 10
sum = notPurePlus(2, 3)
print(myFavoriteState) # > 9999
```

Wait, what? 9999? Argh! Surely `notPurePlus` must have mutated my state!

---

## Pure

> Pure functions' only effect is computing the return value.

```python
myFavoriteState = 10
sum = purePlus(2, 3)
print(myFavoriteState) # > 10
```

😌😌😌😌😌😌😌😌😌😌😌😌😌😌

---

## Pure

> Pure functions' only effect is computing the return value.

Benefit:

- You can trust that the function only computes a value and does not affect anything else.
- With shared state limited, little state management => obvious where one can use concurrency.

---

# OK, cool. Sounds utopic. How?

---

## Let's loop back

Several "levels" of how:

- ✅ ~~Functional principles~~ (total, deterministic, pure)
- **Functional language features in mainly imperative languages**
- Functional or semi-functional languages

---

## Higher-order functions

---

## Higher-order functions

Functions that take functions as parameters.

```python
def performMathematicalOperationAndAdd2(operation, numberToPerformOn):
  result = operation(numberToPerformOn)
  return result + 2
  
def main():
  def square(number):
    return number * number
  
  result = performMathematicalOperation(square, 4)
  print(result) # > 18

```

---

## Higher-order functions

Some important built-ins you'll find in most languages:

- `map` (`std::transform` in C++)
- `filter` (`std::remove_if` in C++)
- `reduce` (a special case of `fold` if you can't find `reduce` in your language)

---

## Higher-order functions - map

`map` takes two parameters:

1. A container (i.e. an `array`) of value(s)
2. A function that takes one such value and returns some other value.

`map` then returns:
A new container where all the values from `1` have been passed through the function.

---

## Higher-order functions – map

js:

```javascript
function plusOne(number) {
  return number + 1
}
const myList = [1,2,3]

myList.map(plusOne) // > [2,3,4]

```

python:

```python

def plusOne(number) {
  return number + 1
}
myList = [1,2,3]

map(plusOne, myList) # >[2,3,4]

```

^ Note that the map function does not mutate the original list (pure), it will return the same thing every time (deterministic) and always return a value as long as the passed function does not crash. (total)

---

## Higher-order functions – filter

`filter` takes two parameters:

1. A container (i.e. an `array`) of value(s)
2. A function that takes one such value and returns either `true` ("keep me!") or `false` ("Filter me out")

`filter` then returns:
A new container where all the values from `1` have been filtered through the passed function.

---

## Higher-order functions – filter

```python

class Person:
  def __init__(self, name, age):
    self.name = name
    self.age = age

kjetil = Person("Kjetil", 26)
haakon = Person("Håkon", 28)

def isOld(person):
  if person.age > 26:
    return True
  else:
    return False

filter(!isOld, [kjetil, haakon])  # [Person("Kjetil", 26)]

```

---

## Higher-order functions – filter

Functional vs imperative:

```python
arrayToFilter = [kjetil, haakon]

# imperative
filteredArray = []
for element in arrayToFilter:
  if !isOld(element):
    filteredArray.push(element)


# functional
filteredArray = filter(isOld, arrayToFilter)

```

---

## Other ways to get closer to functional bliss – a sneak peek

Some languages, like `Scala`, helps you achieve close to complete totality in your functions, with constructs such as `Option`:

An `Option` either exists: `Some(3)` for instance, or does not exist: `None`.

---

# Other subjects to possibly cover

---

## Other subjects to possibly cover

- More functional techniques in mainly imperative languages (like python and js)
  - Currying (Partly calling a function and receiving a new function)
  - Program design: How to manage state?

---

## Other subjects to possibly cover

### Typed functional programming and its benefits (Scala, Kotlin, Haskell, javascript?):

- Optional types and other powerful higher-order *types*
- Union types
- Powerful domain modeling
- Pattern matching etc.

---

## More advanced topics

- How to handle effects like HTTP and I/O while still being pure, deterministic and total? (IO monads)
- Techniques to do a sequence of pure functions concurrent with a snap of your fingers
- Type classes
- Functional abstractions using type classes
- Monads 😱, Applicatives 😨, Functors 😏

---

# What do we want this guild to be?

- Insert functional principles in our existing codebase
  - Refactoring sessions in an FP context
- Functional principles
- More advanced concepts
  - Workshops and work-together sessions

Every other week!

- Next time:
  - Do something together
  - Find concrete refactoring examples and live-code them functional
  - Non-linear search-repo?

- Next time 2:
  - Category theory
  - How it relates to fp
