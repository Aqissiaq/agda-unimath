---
title: Univalent Mathematics in Agda
---

# Iterating functions

```agda
{-# OPTIONS --without-K --exact-split #-}

module elementary-number-theory.iterating-functions where

open import elementary-number-theory.addition-natural-numbers using
  ( add-ℕ; commutative-add-ℕ)
open import elementary-number-theory.natural-numbers using (ℕ; zero-ℕ; succ-ℕ)
open import foundation.identity-types using (Id; refl; _∙_; inv; ap)
open import foundation.universe-levels using (Level; UU)
```

# Iterating functions

```agda
module _
  {l : Level} {X : UU l}
  where
  
  iterate : ℕ → (X → X) → (X → X)
  iterate zero-ℕ f x = x
  iterate (succ-ℕ k) f x = f (iterate k f x)

  iterate-succ-ℕ :
    (k : ℕ) (f : X → X) (x : X) →
    Id (iterate (succ-ℕ k) f x) (iterate k f (f x))
  iterate-succ-ℕ zero-ℕ f x = refl
  iterate-succ-ℕ (succ-ℕ k) f x = ap f (iterate-succ-ℕ k f x)

  iterate-add-ℕ :
    (k l : ℕ) (f : X → X) (x : X) →
    Id (iterate (add-ℕ k l) f x) (iterate k f (iterate l f x))
  iterate-add-ℕ k zero-ℕ f x = refl
  iterate-add-ℕ k (succ-ℕ l) f x =
    ap f (iterate-add-ℕ k l f x) ∙ iterate-succ-ℕ k f (iterate l f x)

  iterate-iterate :
    (k l : ℕ) (f : X → X) (x : X) →
    Id (iterate k f (iterate l f x)) (iterate l f (iterate k f x))
  iterate-iterate k l f x =
    ( inv (iterate-add-ℕ k l f x)) ∙
    ( ( ap (λ t → iterate t f x) (commutative-add-ℕ k l)) ∙
      ( iterate-add-ℕ l k f x))
```
