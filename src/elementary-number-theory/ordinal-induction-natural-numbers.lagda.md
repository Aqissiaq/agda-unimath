# The ordinal induction principle for the natural numbers

```agda
{-# OPTIONS --without-K --exact-split #-}

module elementary-number-theory.ordinal-induction-natural-numbers where

open import elementary-number-theory.inequality-natural-numbers using
  ( le-ℕ; succ-le-ℕ; contradiction-le-zero-ℕ; transitive-le-ℕ')
open import elementary-number-theory.natural-numbers using (ℕ; zero-ℕ; succ-ℕ)

open import foundation.empty-types using (ex-falso)
open import foundation.universe-levels using (Level; UU)
```

## Idea

The ordinal induction principle of the natural numbers is the well-founded induction principle of ℕ.

## To Do

The computation rule should still be proven.

```agda
□-<-ℕ :
  {l : Level} → (ℕ → UU l) → ℕ → UU l
□-<-ℕ P n = (m : ℕ) → (le-ℕ m n) → P m

reflect-□-<-ℕ :
  {l : Level} (P : ℕ → UU l) →
  (( n : ℕ) → □-<-ℕ P n) → (n : ℕ) → P n
reflect-□-<-ℕ P f n = f (succ-ℕ n) n (succ-le-ℕ n)

zero-ordinal-ind-ℕ :
  { l : Level} (P : ℕ → UU l) → □-<-ℕ P zero-ℕ
zero-ordinal-ind-ℕ P m t = ex-falso (contradiction-le-zero-ℕ m t)

succ-ordinal-ind-ℕ :
  {l : Level} (P : ℕ → UU l) → ((n : ℕ) → (□-<-ℕ P n) → P n) →
  (k : ℕ) → □-<-ℕ P k → □-<-ℕ P (succ-ℕ k)
succ-ordinal-ind-ℕ P f k g m t =
  f m (λ m' t' → g m' (transitive-le-ℕ' m' m k t' t))

induction-ordinal-ind-ℕ :
  { l : Level} (P : ℕ → UU l) →
  ( qS : (k : ℕ) → □-<-ℕ P k → □-<-ℕ P (succ-ℕ k))
  ( n : ℕ) → □-<-ℕ P n
induction-ordinal-ind-ℕ P qS zero-ℕ = zero-ordinal-ind-ℕ P 
induction-ordinal-ind-ℕ P qS (succ-ℕ n) =
  qS n (induction-ordinal-ind-ℕ P qS n)

ordinal-ind-ℕ :
  { l : Level} (P : ℕ → UU l) →
  ( (n : ℕ) → (□-<-ℕ P n) → P n) →
  ( n : ℕ) → P n
ordinal-ind-ℕ P f =
  reflect-□-<-ℕ P
    ( induction-ordinal-ind-ℕ P (succ-ordinal-ind-ℕ P f))
```
