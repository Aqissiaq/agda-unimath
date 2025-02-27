# Prime numbers

```agda
{-# OPTIONS --without-K --exact-split #-}

module elementary-number-theory.prime-numbers where

open import elementary-number-theory.decidable-dependent-function-types using
  ( is-decidable-bounded-Π-ℕ)
open import elementary-number-theory.divisibility-natural-numbers using
  ( div-one-ℕ; leq-div-succ-ℕ)
open import elementary-number-theory.equality-natural-numbers using
  ( is-decidable-is-one-ℕ; Eq-eq-ℕ; is-set-ℕ)
open import elementary-number-theory.multiplication-natural-numbers using
  ( right-zero-law-mul-ℕ)
open import elementary-number-theory.natural-numbers using
  ( ℕ; zero-ℕ; succ-ℕ; is-one-ℕ; is-not-one-ℕ; is-not-one-two-ℕ)
open import elementary-number-theory.proper-divisors-natural-numbers using
  ( is-proper-divisor-ℕ; is-proper-divisor-zero-succ-ℕ;
    is-decidable-is-proper-divisor-ℕ; is-prop-is-proper-divisor-ℕ;
    is-proper-divisor-one-is-proper-divisor-ℕ)
    
open import foundation.cartesian-product-types using (_×_)
open import foundation.contractible-types using (is-contr; eq-is-contr; center)
open import foundation.coproduct-types using (inl; inr)
open import foundation.decidable-types using
  ( is-decidable; is-decidable-prod; is-decidable-neg; is-decidable-iff)
open import foundation.dependent-pair-types using (Σ; pair; pr1; pr2)
open import foundation.empty-types using (ex-falso)
open import foundation.functions using (_∘_)
open import foundation.fundamental-theorem-of-identity-types using
  ( fundamental-theorem-id; fundamental-theorem-id')
open import foundation.identity-types using (refl; inv; _∙_; ap)
open import foundation.logical-equivalences using (_↔_; iff-equiv)
open import foundation.propositions using (is-equiv-is-prop)
open import foundation.universe-levels using (UU; lzero)
```

## Idea

A prime number is a natural number of which 1 is the only proper divisor.

## Definition

### Definition 1

This is a direct interpretation of what it means to be prime.

```agda
is-prime-ℕ : ℕ → UU lzero
is-prime-ℕ n = (x : ℕ) → (is-proper-divisor-ℕ n x ↔ is-one-ℕ x)
```

### Definition 2

This is an implementation of the idea of being prime, which is usually taken as the definition.

```agda
is-one-is-proper-divisor-ℕ : ℕ → UU lzero
is-one-is-proper-divisor-ℕ n =
  (x : ℕ) → is-proper-divisor-ℕ n x → is-one-ℕ x

is-prime-easy-ℕ : ℕ → UU lzero
is-prime-easy-ℕ n = (is-not-one-ℕ n) × (is-one-is-proper-divisor-ℕ n)
```

### Definition 3

```agda
has-unique-proper-divisor-ℕ : ℕ → UU lzero
has-unique-proper-divisor-ℕ n = is-contr (Σ ℕ (is-proper-divisor-ℕ n))
```

## Properties

### Definitions 1, 2, and 3 of prime numbers are equivalent

```agda
is-not-one-is-prime-ℕ : (n : ℕ) → is-prime-ℕ n → is-not-one-ℕ n
is-not-one-is-prime-ℕ n H p = pr1 (pr2 (H 1) refl) (inv p)

is-prime-easy-is-prime-ℕ : (n : ℕ) → is-prime-ℕ n → is-prime-easy-ℕ n
pr1 (is-prime-easy-is-prime-ℕ n H) = is-not-one-is-prime-ℕ n H
pr2 (is-prime-easy-is-prime-ℕ n H) x = pr1 (H x)

is-prime-is-prime-easy-ℕ : (n : ℕ) → is-prime-easy-ℕ n → is-prime-ℕ n
pr1 (is-prime-is-prime-easy-ℕ n H x) = pr2 H x
pr1 (pr2 (is-prime-is-prime-easy-ℕ n H .(succ-ℕ zero-ℕ)) refl) q = pr1 H (inv q)
pr2 (pr2 (is-prime-is-prime-easy-ℕ n H .(succ-ℕ zero-ℕ)) refl) = div-one-ℕ n

has-unique-proper-divisor-is-prime-ℕ :
  (n : ℕ) → is-prime-ℕ n → has-unique-proper-divisor-ℕ n
has-unique-proper-divisor-is-prime-ℕ n H =
  fundamental-theorem-id' 1
    ( pr2 (H 1) refl)
    ( λ x p → pr2 (H x) (inv p))
    ( λ x → is-equiv-is-prop (is-set-ℕ 1 x) (is-prop-is-proper-divisor-ℕ n x) (λ p → inv (pr1 (H x) p)))

is-prime-has-unique-proper-divisor-ℕ :
  (n : ℕ) → has-unique-proper-divisor-ℕ n → is-prime-ℕ n
pr1 (is-prime-has-unique-proper-divisor-ℕ n H x) K =
  ap pr1
    ( eq-is-contr H
      { pair x K}
      { pair 1 (is-proper-divisor-one-is-proper-divisor-ℕ K)})
pr2 (is-prime-has-unique-proper-divisor-ℕ n H .1) refl =
  is-proper-divisor-one-is-proper-divisor-ℕ (pr2 (center H))
```

### Being a prime is decidable

```agda
is-decidable-is-prime-easy-ℕ : (n : ℕ) → is-decidable (is-prime-easy-ℕ n)
is-decidable-is-prime-easy-ℕ zero-ℕ =
  inr
    ( λ H →
      is-not-one-two-ℕ (pr2 H 2 (is-proper-divisor-zero-succ-ℕ 1)))
is-decidable-is-prime-easy-ℕ (succ-ℕ n) =
  is-decidable-prod
    ( is-decidable-neg (is-decidable-is-one-ℕ (succ-ℕ n)))
    ( is-decidable-bounded-Π-ℕ
      ( is-proper-divisor-ℕ (succ-ℕ n))
      ( is-one-ℕ)
      ( is-decidable-is-proper-divisor-ℕ (succ-ℕ n))
      ( is-decidable-is-one-ℕ)
      ( succ-ℕ n)
      ( λ x H → leq-div-succ-ℕ x n (pr2 H)))

is-decidable-is-prime-ℕ : (n : ℕ) → is-decidable (is-prime-ℕ n)
is-decidable-is-prime-ℕ n =
  is-decidable-iff
    ( is-prime-is-prime-easy-ℕ n)
    ( is-prime-easy-is-prime-ℕ n)
    ( is-decidable-is-prime-easy-ℕ n)
```

## Examples

### The number 2 is a prime.

```agda
is-one-is-proper-divisor-two-ℕ : is-one-is-proper-divisor-ℕ 2
is-one-is-proper-divisor-two-ℕ zero-ℕ (pair f (pair k p)) =
  ex-falso (f (inv (right-zero-law-mul-ℕ k) ∙ p))
is-one-is-proper-divisor-two-ℕ (succ-ℕ zero-ℕ) (pair f H) = refl
is-one-is-proper-divisor-two-ℕ (succ-ℕ (succ-ℕ zero-ℕ)) (pair f H) =
  ex-falso (f refl)
is-one-is-proper-divisor-two-ℕ (succ-ℕ (succ-ℕ (succ-ℕ x))) (pair f H) =
  ex-falso (leq-div-succ-ℕ (succ-ℕ (succ-ℕ (succ-ℕ x))) 1 H)
  
is-prime-easy-two-ℕ : is-prime-easy-ℕ 2
pr1 is-prime-easy-two-ℕ = Eq-eq-ℕ
pr2 is-prime-easy-two-ℕ = is-one-is-proper-divisor-two-ℕ

is-prime-two-ℕ : is-prime-ℕ 2
is-prime-two-ℕ =
  is-prime-is-prime-easy-ℕ 2 is-prime-easy-two-ℕ
```
