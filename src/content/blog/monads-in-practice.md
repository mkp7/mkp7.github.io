---
title: 'Monads In Practice'
description: '--'
pubDate: 'May 29 2025'
heroImage: '/blog-placeholder-2.jpg'
---

### Monoid:
```scala
trait Monoid[A]:
  def identity: A
  def append(a: A, b: A): A
```
#### Properties:
- **Identity:** *no-op* value. It doesn't do anything when used with `append` method.
- **Associativity:** The order in which operations (`append`) are nested doesn’t matter—as long as the sequence of function applications is preserved. This allows chaining multiple computations reliably.

### Functor:
```scala
trait Functor[F[_]]:
  def map[A, B](a: F[A])(fn: A => B): F[B]
```
#### Properties:
- **Identity:** *no-op* value. It doesn't do anything when used with `map` method.
- **Composition:** Grouping of multiple `map` methods isn't going change the end result.

### Monad:
```scala
trait Monad[M[_]]:
  def pure[A](a: A): M[A]
  def flatMap[A, B](a: M[A])(fn: A => M[B]): M[B]
  def map[A, B](a: M[A])(fn: A => B): M[B] =
	flatMap(a)((b: A) => pure(fn(b)))
  def append[A, B, C](
	f1: A => M[B],
	f2: B => M[C]
  ): A => M[C] =
    (a: A) => flatMap(f1(a))(f2)
```
#### Properties:
- **Identity:** *no-op* value. It doesn't do anything when used with `flatMap` method.
- **Composition:** Grouping of multiple `flatMap` methods isn't going change the end result.

> The term **monad** is a bit cryptic if you are not a mathematician.
> An alternative term is **computation builder** which is a bit more descriptive of what they are actually useful for in practice.

**Monad** is a pattern for chaining operations. It looks a bit like method chaining in OOP, but the mechanism is slightly different.