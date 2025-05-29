---
layout: ../../layouts/BlogLayout.astro
title: Monads In Practice
description: 'What are Monads? And how to use them?'
timestamp: 2025-05-29
time: 3
filename: monads-in-practice
featured: true
---


> The term **monad** is a bit cryptic if you are not a mathematician.

> An alternative term is **computation builder** which is a bit more descriptive of what they are actually useful for in practice.

### Monoid:
```scala
trait Monoid[A]:
  def identity: A
  def append(a: A, b: A): A
```
#### Within a `Monoid`:
- `A` defines a type for a value.
- `identity` is a value of type `A`.
- `append` is a function which
	- takes two arguments of the same type `A`
	- returns the value of same type `A`
#### Properties:
- **Identity:** A *no-op* value. It doesn't do anything when used with `append` method.
- **Associativity:** The order in which operations (`append`) are nested doesn’t matter—as long as the sequence of function applications is preserved. This allows chaining multiple computations reliably.


### Functor:
```scala
trait Functor[F[_]]:
  def map[A, B](a: F[A])(fn: A => B): F[B]
```
#### Within a `Functor`:
- `F[_]` defines a context of type `F` for some value of type `_`.
- For example: `F[A]` defines a context of type `F` for a value of type `A`.
- `map` is a function which
	- takes an argument of type `F[A]`
	- takes a function which
		- accepts a value of type `A`
		- returns a value of type `B`
	- returns the value of type `F[B]`
- `map` transforms a value of type  `F[A]` to value of type `F[B]`.
#### Properties:
- **Identity:** A *no-op* value. It doesn't do anything when used with `map` method.
- **Composition:** Grouping of multiple `map` methods isn't going change the end result.


### Monad:
```scala
trait Monad[M[_]]:
  def pure[A](a: A): M[A]
  
  def flatMap[A, B](a: M[A])(fn: A => M[B]): M[B]
  
  def map[A, B](a: M[A])(fn: A => B): M[B] =
	flatMap(a)((b: A) => pure(fn(b)))
  
  def append[A, B, C](f1: A => M[B], f2: B => M[C]): A => M[C] =
    (a: A) => flatMap(f1(a))(f2)
```
#### Within a `Monad`:
- `M[_]` defines a context of type `M` for some value of type `_`.
- For example: `M[A]` defines a context of type `M` for a value of type `A`.
- `pure` is a function which
	- takes an arguments of type `A`
	- returns a value of type `M[A]`
- `flatMap` is a function which
	- takes an argument of type `M[A]`
	- takes a function which
		- accepts a value of type `A`
		- returns a value of type `M[B]` (notice how it differs here compare to `map` function above)
	- which returns the value of type `M[B]`
- `flatMap` transforms a value of type  `M[A]` to value of type `M[B]`.
#### Properties:
- **Identity:** A *no-op* value. It doesn't do anything when used with `flatMap` method.
- **Composition:** Grouping of multiple `flatMap` methods isn't going change the end result.

> **Monad** is a pattern for chaining operations. It looks a bit like method chaining in OOP, but the mechanism is slightly different.

To be continued...