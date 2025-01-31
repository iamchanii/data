---
title: Predicate.ts
nav_order: 35
parent: Modules
---

## Predicate overview

Added in v1.0.0

---

<h2 class="text-delta">Table of contents</h2>

- [combinators](#combinators)
  - [and](#and)
  - [eqv](#eqv)
  - [implies](#implies)
  - [nand](#nand)
  - [nor](#nor)
  - [not](#not)
  - [or](#or)
  - [xor](#xor)
- [constructors](#constructors)
  - [contramap](#contramap)
- [do notation](#do-notation)
  - [Do](#do)
  - [bindDiscard](#binddiscard)
  - [bindTo](#bindto)
- [guards](#guards)
  - [isBigint](#isbigint)
  - [isBoolean](#isboolean)
  - [isDate](#isdate)
  - [isError](#iserror)
  - [isFunction](#isfunction)
  - [isNever](#isnever)
  - [isNotNull](#isnotnull)
  - [isNotNullable](#isnotnullable)
  - [isNotUndefined](#isnotundefined)
  - [isNull](#isnull)
  - [isNullable](#isnullable)
  - [isNumber](#isnumber)
  - [isObject](#isobject)
  - [isReadonlyRecord](#isreadonlyrecord)
  - [isRecord](#isrecord)
  - [isString](#isstring)
  - [isSymbol](#issymbol)
  - [isUndefined](#isundefined)
  - [isUnknown](#isunknown)
- [instances](#instances)
  - [Contravariant](#contravariant)
  - [Invariant](#invariant)
  - [Product](#product)
  - [SemiProduct](#semiproduct)
  - [getMonoidEqv](#getmonoideqv)
  - [getMonoidEvery](#getmonoidevery)
  - [getMonoidSome](#getmonoidsome)
  - [getMonoidXor](#getmonoidxor)
  - [getSemigroupEqv](#getsemigroupeqv)
  - [getSemigroupEvery](#getsemigroupevery)
  - [getSemigroupSome](#getsemigroupsome)
  - [getSemigroupXor](#getsemigroupxor)
- [models](#models)
  - [Predicate (interface)](#predicate-interface)
  - [Refinement (interface)](#refinement-interface)
- [type lambdas](#type-lambdas)
  - [PredicateTypeLambda (interface)](#predicatetypelambda-interface)
- [utils](#utils)
  - [appendElement](#appendelement)
  - [compose](#compose)
  - [every](#every)
  - [some](#some)
  - [struct](#struct)
  - [tuple](#tuple)
  - [tupled](#tupled)

---

# combinators

## and

Combines two predicates into a new predicate that returns `true` if both of the predicates returns `true`.

**Signature**

```ts
export declare const and: {
  <A, C extends A>(that: Refinement<A, C>): <B extends A>(self: Refinement<A, B>) => Refinement<A, B & C>
  <A, B extends A, C extends A>(self: Refinement<A, B>, that: Refinement<A, C>): Refinement<A, B & C>
  <A>(that: Predicate<A>): (self: Predicate<A>) => Predicate<A>
  <A>(self: Predicate<A>, that: Predicate<A>): Predicate<A>
}
```

**Example**

```ts
import * as P from '@effect/data/Predicate'

const minLength = (n: number) => (s: string) => s.length >= n
const maxLength = (n: number) => (s: string) => s.length <= n

const length = (n: number) => P.and(minLength(n), maxLength(n))

assert.deepStrictEqual(length(2)('aa'), true)
assert.deepStrictEqual(length(2)('a'), false)
assert.deepStrictEqual(length(2)('aaa'), false)
```

Added in v1.0.0

## eqv

**Signature**

```ts
export declare const eqv: {
  <A>(that: Predicate<A>): (self: Predicate<A>) => Predicate<A>
  <A>(self: Predicate<A>, that: Predicate<A>): Predicate<A>
}
```

Added in v1.0.0

## implies

**Signature**

```ts
export declare const implies: {
  <A>(that: Predicate<A>): (self: Predicate<A>) => Predicate<A>
  <A>(self: Predicate<A>, that: Predicate<A>): Predicate<A>
}
```

Added in v1.0.0

## nand

**Signature**

```ts
export declare const nand: {
  <A>(that: Predicate<A>): (self: Predicate<A>) => Predicate<A>
  <A>(self: Predicate<A>, that: Predicate<A>): Predicate<A>
}
```

Added in v1.0.0

## nor

**Signature**

```ts
export declare const nor: {
  <A>(that: Predicate<A>): (self: Predicate<A>) => Predicate<A>
  <A>(self: Predicate<A>, that: Predicate<A>): Predicate<A>
}
```

Added in v1.0.0

## not

Negates the result of a given predicate.

**Signature**

```ts
export declare const not: <A>(self: Predicate<A>) => Predicate<A>
```

**Example**

```ts
import * as P from '@effect/data/Predicate'
import * as N from '@effect/data/Number'

const isPositive = P.not(N.lessThan(0))

assert.deepStrictEqual(isPositive(-1), false)
assert.deepStrictEqual(isPositive(0), true)
assert.deepStrictEqual(isPositive(1), true)
```

Added in v1.0.0

## or

Combines two predicates into a new predicate that returns `true` if at least one of the predicates returns `true`.

**Signature**

```ts
export declare const or: {
  <A>(that: Predicate<A>): (self: Predicate<A>) => Predicate<A>
  <A>(self: Predicate<A>, that: Predicate<A>): Predicate<A>
}
```

**Example**

```ts
import * as P from '@effect/data/Predicate'
import * as N from '@effect/data/Number'

const nonZero = P.or(N.lessThan(0), N.greaterThan(0))

assert.deepStrictEqual(nonZero(-1), true)
assert.deepStrictEqual(nonZero(0), false)
assert.deepStrictEqual(nonZero(1), true)
```

Added in v1.0.0

## xor

**Signature**

```ts
export declare const xor: {
  <A>(that: Predicate<A>): (self: Predicate<A>) => Predicate<A>
  <A>(self: Predicate<A>, that: Predicate<A>): Predicate<A>
}
```

Added in v1.0.0

# constructors

## contramap

Given a `Predicate<A>` returns a `Predicate<B>`

**Signature**

```ts
export declare const contramap: {
  <B, A>(f: (b: B) => A): (self: Predicate<A>) => Predicate<B>
  <A, B>(self: Predicate<A>, f: (b: B) => A): Predicate<B>
}
```

**Example**

```ts
import * as P from '@effect/data/Predicate'
import * as N from '@effect/data/Number'

const minLength3 = P.contramap(N.greaterThan(2), (s: string) => s.length)

assert.deepStrictEqual(minLength3('a'), false)
assert.deepStrictEqual(minLength3('aa'), false)
assert.deepStrictEqual(minLength3('aaa'), true)
assert.deepStrictEqual(minLength3('aaaa'), true)
```

Added in v1.0.0

# do notation

## Do

**Signature**

```ts
export declare const Do: () => Predicate<{}>
```

Added in v1.0.0

## bindDiscard

A variant of `bind` that sequentially ignores the scope.

**Signature**

```ts
export declare const bindDiscard: {
  <N extends string, A extends object, B>(name: Exclude<N, keyof A>, that: Predicate<B>): (
    self: Predicate<A>
  ) => Predicate<{ readonly [K in N | keyof A]: K extends keyof A ? A[K] : B }>
  <A extends object, N extends string, B>(
    self: Predicate<A>,
    name: Exclude<N, keyof A>,
    that: Predicate<B>
  ): Predicate<{ readonly [K in N | keyof A]: K extends keyof A ? A[K] : B }>
}
```

Added in v1.0.0

## bindTo

**Signature**

```ts
export declare const bindTo: {
  <N extends string>(name: N): <A>(self: Predicate<A>) => Predicate<{ readonly [K in N]: A }>
  <A, N extends string>(self: Predicate<A>, name: N): Predicate<{ readonly [K in N]: A }>
}
```

Added in v1.0.0

# guards

## isBigint

Tests if a value is a `bigint`.

**Signature**

```ts
export declare const isBigint: (input: unknown) => input is bigint
```

**Example**

```ts
import { isBigint } from '@effect/data/Predicate'

assert.deepStrictEqual(isBigint(1n), true)

assert.deepStrictEqual(isBigint(1), false)
```

Added in v1.0.0

## isBoolean

Tests if a value is a `boolean`.

**Signature**

```ts
export declare const isBoolean: (input: unknown) => input is boolean
```

**Example**

```ts
import { isBoolean } from '@effect/data/Predicate'

assert.deepStrictEqual(isBoolean(true), true)

assert.deepStrictEqual(isBoolean('true'), false)
```

Added in v1.0.0

## isDate

A guard that succeeds when the input is a `Date`.

**Signature**

```ts
export declare const isDate: (input: unknown) => input is Date
```

**Example**

```ts
import { isDate } from '@effect/data/Predicate'

assert.deepStrictEqual(isDate(new Date()), true)

assert.deepStrictEqual(isDate(null), false)
assert.deepStrictEqual(isDate({}), false)
```

Added in v1.0.0

## isError

A guard that succeeds when the input is an `Error`.

**Signature**

```ts
export declare const isError: (input: unknown) => input is Error
```

**Example**

```ts
import { isError } from '@effect/data/Predicate'

assert.deepStrictEqual(isError(new Error()), true)

assert.deepStrictEqual(isError(null), false)
assert.deepStrictEqual(isError({}), false)
```

Added in v1.0.0

## isFunction

Tests if a value is a `function`.

**Signature**

```ts
export declare const isFunction: (input: unknown) => input is Function
```

**Example**

```ts
import { isFunction } from '@effect/data/Predicate'

assert.deepStrictEqual(isFunction(isFunction), true)

assert.deepStrictEqual(isFunction('function'), false)
```

Added in v1.0.0

## isNever

A guard that always fails.

**Signature**

```ts
export declare const isNever: (input: unknown) => input is never
```

**Example**

```ts
import { isNever } from '@effect/data/Predicate'

assert.deepStrictEqual(isNever(null), false)
assert.deepStrictEqual(isNever(undefined), false)
assert.deepStrictEqual(isNever({}), false)
assert.deepStrictEqual(isNever([]), false)
```

Added in v1.0.0

## isNotNull

Tests if a value is not `undefined`.

**Signature**

```ts
export declare const isNotNull: <A>(input: A) => input is Exclude<A, null>
```

**Example**

```ts
import { isNotNull } from '@effect/data/Predicate'

assert.deepStrictEqual(isNotNull(undefined), true)
assert.deepStrictEqual(isNotNull('null'), true)

assert.deepStrictEqual(isNotNull(null), false)
```

Added in v1.0.0

## isNotNullable

A guard that succeeds when the input is not `null` or `undefined`.

**Signature**

```ts
export declare const isNotNullable: <A>(input: A) => input is NonNullable<A>
```

**Example**

```ts
import { isNotNullable } from '@effect/data/Predicate'

assert.deepStrictEqual(isNotNullable({}), true)
assert.deepStrictEqual(isNotNullable([]), true)

assert.deepStrictEqual(isNotNullable(null), false)
assert.deepStrictEqual(isNotNullable(undefined), false)
```

Added in v1.0.0

## isNotUndefined

Tests if a value is not `undefined`.

**Signature**

```ts
export declare const isNotUndefined: <A>(input: A) => input is Exclude<A, undefined>
```

**Example**

```ts
import { isNotUndefined } from '@effect/data/Predicate'

assert.deepStrictEqual(isNotUndefined(null), true)
assert.deepStrictEqual(isNotUndefined('undefined'), true)

assert.deepStrictEqual(isNotUndefined(undefined), false)
```

Added in v1.0.0

## isNull

Tests if a value is `undefined`.

**Signature**

```ts
export declare const isNull: (input: unknown) => input is null
```

**Example**

```ts
import { isNull } from '@effect/data/Predicate'

assert.deepStrictEqual(isNull(null), true)

assert.deepStrictEqual(isNull(undefined), false)
assert.deepStrictEqual(isNull('null'), false)
```

Added in v1.0.0

## isNullable

A guard that succeeds when the input is `null` or `undefined`.

**Signature**

```ts
export declare const isNullable: <A>(input: A) => input is Extract<A, null | undefined>
```

**Example**

```ts
import { isNullable } from '@effect/data/Predicate'

assert.deepStrictEqual(isNullable(null), true)
assert.deepStrictEqual(isNullable(undefined), true)

assert.deepStrictEqual(isNullable({}), false)
assert.deepStrictEqual(isNullable([]), false)
```

Added in v1.0.0

## isNumber

Tests if a value is a `number`.

**Signature**

```ts
export declare const isNumber: (input: unknown) => input is number
```

**Example**

```ts
import { isNumber } from '@effect/data/Predicate'

assert.deepStrictEqual(isNumber(2), true)

assert.deepStrictEqual(isNumber('2'), false)
```

Added in v1.0.0

## isObject

Tests if a value is an `object`.

**Signature**

```ts
export declare const isObject: (input: unknown) => input is object
```

**Example**

```ts
import { isObject } from '@effect/data/Predicate'

assert.deepStrictEqual(isObject({}), true)
assert.deepStrictEqual(isObject([]), true)

assert.deepStrictEqual(isObject(null), false)
assert.deepStrictEqual(isObject(undefined), false)
```

Added in v1.0.0

## isReadonlyRecord

A guard that succeeds when the input is a readonly record.

**Signature**

```ts
export declare const isReadonlyRecord: (
  input: unknown
) => input is { readonly [x: string]: unknown; readonly [x: symbol]: unknown }
```

**Example**

```ts
import { isReadonlyRecord } from '@effect/data/Predicate'

assert.deepStrictEqual(isReadonlyRecord({}), true)
assert.deepStrictEqual(isReadonlyRecord({ a: 1 }), true)

assert.deepStrictEqual(isReadonlyRecord([]), false)
assert.deepStrictEqual(isReadonlyRecord([1, 2, 3]), false)
assert.deepStrictEqual(isReadonlyRecord(null), false)
assert.deepStrictEqual(isReadonlyRecord(undefined), false)
```

Added in v1.0.0

## isRecord

A guard that succeeds when the input is a record.

**Signature**

```ts
export declare const isRecord: (input: unknown) => input is { [x: string]: unknown; [x: symbol]: unknown }
```

**Example**

```ts
import { isRecord } from '@effect/data/Predicate'

assert.deepStrictEqual(isRecord({}), true)
assert.deepStrictEqual(isRecord({ a: 1 }), true)

assert.deepStrictEqual(isRecord([]), false)
assert.deepStrictEqual(isRecord([1, 2, 3]), false)
assert.deepStrictEqual(isRecord(null), false)
assert.deepStrictEqual(isRecord(undefined), false)
```

Added in v1.0.0

## isString

Tests if a value is a `string`.

**Signature**

```ts
export declare const isString: (input: unknown) => input is string
```

**Example**

```ts
import { isString } from '@effect/data/Predicate'

assert.deepStrictEqual(isString('a'), true)

assert.deepStrictEqual(isString(1), false)
```

Added in v1.0.0

## isSymbol

Tests if a value is a `symbol`.

**Signature**

```ts
export declare const isSymbol: (input: unknown) => input is symbol
```

**Example**

```ts
import { isSymbol } from '@effect/data/Predicate'

assert.deepStrictEqual(isSymbol(Symbol.for('a')), true)

assert.deepStrictEqual(isSymbol('a'), false)
```

Added in v1.0.0

## isUndefined

Tests if a value is `undefined`.

**Signature**

```ts
export declare const isUndefined: (input: unknown) => input is undefined
```

**Example**

```ts
import { isUndefined } from '@effect/data/Predicate'

assert.deepStrictEqual(isUndefined(undefined), true)

assert.deepStrictEqual(isUndefined(null), false)
assert.deepStrictEqual(isUndefined('undefined'), false)
```

Added in v1.0.0

## isUnknown

A guard that always succeeds.

**Signature**

```ts
export declare const isUnknown: (input: unknown) => input is unknown
```

**Example**

```ts
import { isUnknown } from '@effect/data/Predicate'

assert.deepStrictEqual(isUnknown(null), true)
assert.deepStrictEqual(isUnknown(undefined), true)

assert.deepStrictEqual(isUnknown({}), true)
assert.deepStrictEqual(isUnknown([]), true)
```

Added in v1.0.0

# instances

## Contravariant

**Signature**

```ts
export declare const Contravariant: contravariant.Contravariant<PredicateTypeLambda>
```

Added in v1.0.0

## Invariant

**Signature**

```ts
export declare const Invariant: invariant.Invariant<PredicateTypeLambda>
```

Added in v1.0.0

## Product

**Signature**

```ts
export declare const Product: product_.Product<PredicateTypeLambda>
```

Added in v1.0.0

## SemiProduct

**Signature**

```ts
export declare const SemiProduct: semiProduct.SemiProduct<PredicateTypeLambda>
```

Added in v1.0.0

## getMonoidEqv

**Signature**

```ts
export declare const getMonoidEqv: <A>() => monoid.Monoid<Predicate<A>>
```

Added in v1.0.0

## getMonoidEvery

**Signature**

```ts
export declare const getMonoidEvery: <A>() => monoid.Monoid<Predicate<A>>
```

Added in v1.0.0

## getMonoidSome

**Signature**

```ts
export declare const getMonoidSome: <A>() => monoid.Monoid<Predicate<A>>
```

Added in v1.0.0

## getMonoidXor

**Signature**

```ts
export declare const getMonoidXor: <A>() => monoid.Monoid<Predicate<A>>
```

Added in v1.0.0

## getSemigroupEqv

**Signature**

```ts
export declare const getSemigroupEqv: <A>() => semigroup.Semigroup<Predicate<A>>
```

Added in v1.0.0

## getSemigroupEvery

**Signature**

```ts
export declare const getSemigroupEvery: <A>() => semigroup.Semigroup<Predicate<A>>
```

Added in v1.0.0

## getSemigroupSome

**Signature**

```ts
export declare const getSemigroupSome: <A>() => semigroup.Semigroup<Predicate<A>>
```

Added in v1.0.0

## getSemigroupXor

**Signature**

```ts
export declare const getSemigroupXor: <A>() => semigroup.Semigroup<Predicate<A>>
```

Added in v1.0.0

# models

## Predicate (interface)

**Signature**

```ts
export interface Predicate<A> {
  (a: A): boolean
}
```

Added in v1.0.0

## Refinement (interface)

**Signature**

```ts
export interface Refinement<A, B extends A> {
  (a: A): a is B
}
```

Added in v1.0.0

# type lambdas

## PredicateTypeLambda (interface)

**Signature**

```ts
export interface PredicateTypeLambda extends TypeLambda {
  readonly type: Predicate<this['Target']>
}
```

Added in v1.0.0

# utils

## appendElement

This function appends a predicate to a tuple-like predicate, allowing you to create a new predicate that includes
the original elements and the new one.

**Signature**

```ts
export declare const appendElement: {
  <A extends readonly any[], B>(self: Predicate<A>, that: Predicate<B>): Predicate<readonly [...A, B]>
  <B>(that: Predicate<B>): <A extends readonly any[]>(self: Predicate<A>) => Predicate<readonly [...A, B]>
}
```

Added in v1.0.0

## compose

**Signature**

```ts
export declare const compose: {
  <A, B extends A, C extends B>(bc: Refinement<B, C>): (ab: Refinement<A, B>) => Refinement<A, C>
  <A, B extends A, C extends B>(ab: Refinement<A, B>, bc: Refinement<B, C>): Refinement<A, C>
}
```

Added in v1.0.0

## every

**Signature**

```ts
export declare const every: <A>(collection: Iterable<Predicate<A>>) => Predicate<A>
```

Added in v1.0.0

## some

**Signature**

```ts
export declare const some: <A>(collection: Iterable<Predicate<A>>) => Predicate<A>
```

Added in v1.0.0

## struct

**Signature**

```ts
export declare const struct: <R extends Record<string, Predicate<any>>>(
  predicates: R
) => Predicate<{ readonly [K in keyof R]: [R[K]] extends [Predicate<infer A>] ? A : never }>
```

Added in v1.0.0

## tuple

Similar to `Promise.all` but operates on `Predicate`s.

```
[Predicate<A>, Predicate<B>, ...] -> Predicate<[A, B, ...]>
```

**Signature**

```ts
export declare const tuple: <T extends readonly Predicate<any>[]>(
  ...predicates: T
) => Predicate<Readonly<{ [I in keyof T]: [T[I]] extends [Predicate<infer A>] ? A : never }>>
```

Added in v1.0.0

## tupled

**Signature**

```ts
export declare const tupled: <A>(self: Predicate<A>) => Predicate<readonly [A]>
```

Added in v1.0.0
