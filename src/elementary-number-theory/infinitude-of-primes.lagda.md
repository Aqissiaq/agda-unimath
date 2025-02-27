# The infinitude of primes

```agda
{-# OPTIONS --without-K --exact-split #-}

module elementary-number-theory.infinitude-of-primes where

open import elementary-number-theory.decidable-dependent-function-types using
  ( is-decidable-bounded-Π-ℕ)
open import elementary-number-theory.divisibility-natural-numbers using
  ( is-one-is-divisor-below-ℕ; div-ℕ; is-zero-is-zero-div-ℕ; is-one-div-ℕ;
    transitive-div-ℕ)
open import elementary-number-theory.equality-natural-numbers using
  ( is-decidable-is-one-ℕ; Eq-eq-ℕ; is-decidable-is-zero-ℕ)
open import elementary-number-theory.factorials using
  ( factorial-ℕ; leq-factorial-ℕ; div-factorial-ℕ)
open import elementary-number-theory.inequality-natural-numbers using
  ( le-ℕ; is-decidable-le-ℕ; leq-ℕ; is-decidable-leq-ℕ; is-zero-leq-zero-ℕ;
    concatenate-leq-le-ℕ; le-succ-ℕ; is-nonzero-le-ℕ; neq-le-ℕ; leq-not-le-ℕ;
    contradiction-le-ℕ)
open import elementary-number-theory.lower-bounds-natural-numbers using
  ( is-lower-bound-ℕ)
open import
  elementary-number-theory.modular-arithmetic-standard-finite-types using
  ( is-decidable-div-ℕ)
open import elementary-number-theory.multiplication-natural-numbers using
  ( right-zero-law-mul-ℕ)
open import elementary-number-theory.natural-numbers using
  ( ℕ; zero-ℕ; succ-ℕ; is-one-ℕ; is-nonzero-succ-ℕ; is-nonzero-ℕ; is-not-one-ℕ;
    is-successor-is-nonzero-ℕ)
open import elementary-number-theory.prime-numbers using
  ( is-one-is-proper-divisor-ℕ; is-prime-ℕ; is-prime-is-prime-easy-ℕ;
    is-prime-two-ℕ; is-decidable-is-prime-ℕ)
open import elementary-number-theory.proper-divisors-natural-numbers using
  ( is-proper-divisor-ℕ; le-is-proper-divisor-ℕ)
open import
  elementary-number-theory.well-ordering-principle-natural-numbers using
  ( minimal-element-ℕ; well-ordering-principle-ℕ)
open import foundation.cartesian-product-types using (_×_)
open import foundation.coproduct-types using (inl; inr)
open import foundation.decidable-types using
  ( is-decidable; is-decidable-prod; is-decidable-function-type)
open import foundation.dependent-pair-types using (Σ; pair; pr1; pr2)
open import foundation.empty-types using (ex-falso)
open import foundation.functions using (id)
open import foundation.identity-types using (inv; _∙_; refl)
open import foundation.negation using (¬)
open import foundation.type-arithmetic-empty-type using
  ( is-empty-left-factor-is-empty-prod)
open import foundation.unit-type using (star)
open import foundation.universe-levels using (UU; lzero)
```

## Idea

We show, using the sieve of Eratosthenes and the well-ordering principle of `ℕ`, that there are infinitely many primes. Consequently we obtain the function that returns for each `n` the `n`-th prime, and we obtain the function that counts the number of primes below a number `n`.

## Definition

We begin by stating the infinitude of primes in type theory.

```agda
Infinitude-Of-Primes-ℕ : UU lzero
Infinitude-Of-Primes-ℕ = (n : ℕ) → Σ ℕ (λ p → is-prime-ℕ p × le-ℕ n p)
```

## Proof

### The sieve of Eratosthenes

```agda
in-sieve-of-eratosthenes-ℕ : ℕ → ℕ → UU lzero
in-sieve-of-eratosthenes-ℕ n a =
  (le-ℕ n a) × (is-one-is-divisor-below-ℕ n a)

le-in-sieve-of-eratosthenes-ℕ :
  (n a : ℕ) → in-sieve-of-eratosthenes-ℕ n a → le-ℕ n a
le-in-sieve-of-eratosthenes-ℕ n a = pr1
```

## Being in the sieve of Eratosthenes is decidable

```agda
is-decidable-in-sieve-of-eratosthenes-ℕ :
  (n a : ℕ) → is-decidable (in-sieve-of-eratosthenes-ℕ n a)
is-decidable-in-sieve-of-eratosthenes-ℕ n a =
  is-decidable-prod
    ( is-decidable-le-ℕ n a)
    ( is-decidable-bounded-Π-ℕ
      ( λ x → leq-ℕ x n)
      ( λ x → div-ℕ x a → is-one-ℕ x)
      ( λ x → is-decidable-leq-ℕ x n)
      ( λ x →
        is-decidable-function-type
          ( is-decidable-div-ℕ x a)
          ( is-decidable-is-one-ℕ x))
      ( n)
      ( λ x → id))
```

### The successor of the `n`-th factorial is in the `n`-th sieve

```agda
in-sieve-of-eratosthenes-succ-factorial-ℕ :
  (n : ℕ) → in-sieve-of-eratosthenes-ℕ n (succ-ℕ (factorial-ℕ n))
pr1 (in-sieve-of-eratosthenes-succ-factorial-ℕ zero-ℕ) = star
pr2 (in-sieve-of-eratosthenes-succ-factorial-ℕ zero-ℕ) x l d =
  ex-falso
    ( Eq-eq-ℕ
      ( is-zero-is-zero-div-ℕ x 2 d (is-zero-leq-zero-ℕ x l)))
pr1 (in-sieve-of-eratosthenes-succ-factorial-ℕ (succ-ℕ n)) =
  concatenate-leq-le-ℕ
    { succ-ℕ n}
    { factorial-ℕ (succ-ℕ n)}
    { succ-ℕ (factorial-ℕ (succ-ℕ n))}
    ( leq-factorial-ℕ (succ-ℕ n))
    ( le-succ-ℕ {factorial-ℕ (succ-ℕ n)})
pr2 (in-sieve-of-eratosthenes-succ-factorial-ℕ (succ-ℕ n)) x l (pair y p) with
  is-decidable-is-zero-ℕ x
... | inl refl =
  ex-falso
    ( is-nonzero-succ-ℕ
      ( factorial-ℕ (succ-ℕ n))
      ( inv p ∙ (right-zero-law-mul-ℕ y)))
... | inr f =
  is-one-div-ℕ x
    ( factorial-ℕ (succ-ℕ n))
    ( div-factorial-ℕ (succ-ℕ n) x l f)
    ( pair y p)
```

### The infinitude of primes

```agda
minimal-element-in-sieve-of-eratosthenes-ℕ :
  (n : ℕ) → minimal-element-ℕ (in-sieve-of-eratosthenes-ℕ n)
minimal-element-in-sieve-of-eratosthenes-ℕ n =
  well-ordering-principle-ℕ
    ( in-sieve-of-eratosthenes-ℕ n)
    ( is-decidable-in-sieve-of-eratosthenes-ℕ n)
    ( pair
      ( succ-ℕ (factorial-ℕ n))
      ( in-sieve-of-eratosthenes-succ-factorial-ℕ n))

larger-prime-ℕ : ℕ → ℕ
larger-prime-ℕ n = pr1 (minimal-element-in-sieve-of-eratosthenes-ℕ n)

in-sieve-of-eratosthenes-larger-prime-ℕ :
  (n : ℕ) → in-sieve-of-eratosthenes-ℕ n (larger-prime-ℕ n)
in-sieve-of-eratosthenes-larger-prime-ℕ n =
  pr1 (pr2 (minimal-element-in-sieve-of-eratosthenes-ℕ n))

is-one-is-divisor-below-larger-prime-ℕ :
  (n : ℕ) → is-one-is-divisor-below-ℕ n (larger-prime-ℕ n)
is-one-is-divisor-below-larger-prime-ℕ n =
  pr2 (in-sieve-of-eratosthenes-larger-prime-ℕ n)

le-larger-prime-ℕ : (n : ℕ) → le-ℕ n (larger-prime-ℕ n)
le-larger-prime-ℕ n = pr1 (in-sieve-of-eratosthenes-larger-prime-ℕ n)

is-nonzero-larger-prime-ℕ : (n : ℕ) → is-nonzero-ℕ (larger-prime-ℕ n)
is-nonzero-larger-prime-ℕ n =
  is-nonzero-le-ℕ n (larger-prime-ℕ n) (le-larger-prime-ℕ n)

is-lower-bound-larger-prime-ℕ :
  (n : ℕ) → is-lower-bound-ℕ (in-sieve-of-eratosthenes-ℕ n) (larger-prime-ℕ n)
is-lower-bound-larger-prime-ℕ n =
  pr2 (pr2 (minimal-element-in-sieve-of-eratosthenes-ℕ n))

is-not-one-larger-prime-ℕ :
  (n : ℕ) → is-nonzero-ℕ n → is-not-one-ℕ (larger-prime-ℕ n)
is-not-one-larger-prime-ℕ n H p with is-successor-is-nonzero-ℕ H
... | pair k refl =
  neq-le-ℕ {1} {larger-prime-ℕ n}
    ( concatenate-leq-le-ℕ {1} {succ-ℕ k} {larger-prime-ℕ n} star
      ( le-larger-prime-ℕ (succ-ℕ k)))
    ( inv p)

not-in-sieve-of-eratosthenes-is-proper-divisor-larger-prime-ℕ :
  (n x : ℕ) → is-proper-divisor-ℕ (larger-prime-ℕ n) x →
  ¬ (in-sieve-of-eratosthenes-ℕ n x)
not-in-sieve-of-eratosthenes-is-proper-divisor-larger-prime-ℕ n x H K =
  ex-falso
    ( contradiction-le-ℕ x (larger-prime-ℕ n)
      ( le-is-proper-divisor-ℕ x (larger-prime-ℕ n)
        ( is-nonzero-larger-prime-ℕ n)
        ( H))
      ( is-lower-bound-larger-prime-ℕ n x K))

is-one-is-proper-divisor-larger-prime-ℕ :
  (n : ℕ) → is-nonzero-ℕ n → is-one-is-proper-divisor-ℕ (larger-prime-ℕ n)
is-one-is-proper-divisor-larger-prime-ℕ n H x (pair f K) =
  is-one-is-divisor-below-larger-prime-ℕ n x
    ( leq-not-le-ℕ n x
      ( is-empty-left-factor-is-empty-prod
        ( not-in-sieve-of-eratosthenes-is-proper-divisor-larger-prime-ℕ n x
          ( pair f K))
        ( λ y l d →
          is-one-is-divisor-below-larger-prime-ℕ n y l
            ( transitive-div-ℕ y x (larger-prime-ℕ n) d K))))
    ( K)

is-prime-larger-prime-ℕ :
  (n : ℕ) → is-nonzero-ℕ n → is-prime-ℕ (larger-prime-ℕ n)
is-prime-larger-prime-ℕ n H =
  is-prime-is-prime-easy-ℕ
    ( larger-prime-ℕ n)
    ( pair
      ( is-not-one-larger-prime-ℕ n H)
      ( is-one-is-proper-divisor-larger-prime-ℕ n H))

infinitude-of-primes-ℕ : Infinitude-Of-Primes-ℕ
infinitude-of-primes-ℕ n with is-decidable-is-zero-ℕ n
... | inl refl = pair 2 (pair is-prime-two-ℕ star)
... | inr H =
  pair
    ( larger-prime-ℕ n)
    ( pair
      ( is-prime-larger-prime-ℕ n H)
      ( le-larger-prime-ℕ n))
```

## Consequences

### The `n`-th prime

```agda
prime-ℕ : ℕ → ℕ
prime-ℕ zero-ℕ = 2
prime-ℕ (succ-ℕ n) = pr1 (infinitude-of-primes-ℕ (prime-ℕ n))
```

### The prime counting function

```agda
prime-counting-ℕ : ℕ → ℕ
prime-counting-ℕ zero-ℕ = zero-ℕ
prime-counting-ℕ (succ-ℕ n) with is-decidable-is-prime-ℕ (succ-ℕ n)
... | inl x = succ-ℕ (prime-counting-ℕ n)
... | inr x = prime-counting-ℕ n
```
