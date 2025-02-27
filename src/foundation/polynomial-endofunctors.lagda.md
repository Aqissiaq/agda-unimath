# Polynomial endofunctors

```agda
{-# OPTIONS --without-K --exact-split --allow-unsolved-metas #-}

module foundation.polynomial-endofunctors where

open import foundation.contractible-types using (is-contr; is-contr-total-path)
open import foundation.dependent-pair-types using (Σ; pair; pr1; pr2)
open import foundation.equivalences using
  ( is-equiv; map-inv-is-equiv; isretr-map-inv-is-equiv)
open import foundation.functions using (_∘_; id)
open import foundation.functoriality-dependent-pair-types using (tot)
open import foundation.fundamental-theorem-of-identity-types using
  ( fundamental-theorem-id)
open import foundation.homotopies using
  ( _~_; refl-htpy; is-contr-total-htpy; _·r_)
open import foundation.identity-types using (Id; tr; refl)
open import foundation.structure-identity-principle using
  ( is-contr-total-Eq-structure)
open import foundation.universe-levels using (Level; UU; _⊔_)
```

## Idea

Given a type `A` equipped with a type family `B` over `A`, the polynomial endofunctor `P A B` is defined by

```md
  X ↦ Σ (x : A), (B x → X)
```

Polynomial endofunctors are important in the study of W-types, and also in the study of combinatorial species.

## Definitions

### The action on types of a polynomial endofunctor

```agda
type-polynomial-endofunctor :
  {l1 l2 l3 : Level} (A : UU l1) (B : A → UU l2) (X : UU l3) →
  UU (l1 ⊔ l2 ⊔ l3)
type-polynomial-endofunctor A B X = Σ A (λ x → B x → X)
```

### The identity type of `type-polynomial-endofunctor`

```agda
module _
  {l1 l2 l3 : Level} {A : UU l1} {B : A → UU l2} {X : UU l3}
  where

  Eq-type-polynomial-endofunctor :
    (x y : type-polynomial-endofunctor A B X) → UU (l1 ⊔ l2 ⊔ l3)
  Eq-type-polynomial-endofunctor x y =
    Σ (Id (pr1 x) (pr1 y)) (λ p → (pr2 x) ~ ((pr2 y) ∘ (tr B p)))

  refl-Eq-type-polynomial-endofunctor :
    (x : type-polynomial-endofunctor A B X) →
    Eq-type-polynomial-endofunctor x x
  refl-Eq-type-polynomial-endofunctor (pair x α) = pair refl refl-htpy

  is-contr-total-Eq-type-polynomial-endofunctor :
    (x : type-polynomial-endofunctor A B X) →
    is-contr
      ( Σ ( type-polynomial-endofunctor A B X)
          ( Eq-type-polynomial-endofunctor x))
  is-contr-total-Eq-type-polynomial-endofunctor (pair x α) =
    is-contr-total-Eq-structure
      ( ( λ (y : A) (β : B y → X) (p : Id x y) → α ~ (β ∘ tr B p)))
      ( is-contr-total-path x)
      ( pair x refl)
      ( is-contr-total-htpy α)

  Eq-type-polynomial-endofunctor-eq :
    (x y : type-polynomial-endofunctor A B X) →
    Id x y → Eq-type-polynomial-endofunctor x y
  Eq-type-polynomial-endofunctor-eq x .x refl =
    refl-Eq-type-polynomial-endofunctor x

  is-equiv-Eq-type-polynomial-endofunctor-eq :
    (x y : type-polynomial-endofunctor A B X) →
    is-equiv (Eq-type-polynomial-endofunctor-eq x y)
  is-equiv-Eq-type-polynomial-endofunctor-eq x =
    fundamental-theorem-id x
      ( refl-Eq-type-polynomial-endofunctor x)
      ( is-contr-total-Eq-type-polynomial-endofunctor x)
      ( Eq-type-polynomial-endofunctor-eq x)

  eq-Eq-type-polynomial-endofunctor :
    (x y : type-polynomial-endofunctor A B X) →
    Eq-type-polynomial-endofunctor x y → Id x y
  eq-Eq-type-polynomial-endofunctor x y =
    map-inv-is-equiv (is-equiv-Eq-type-polynomial-endofunctor-eq x y)

  isretr-eq-Eq-type-polynomial-endofunctor :
    (x y : type-polynomial-endofunctor A B X) →
    ( ( eq-Eq-type-polynomial-endofunctor x y) ∘
      ( Eq-type-polynomial-endofunctor-eq x y)) ~ id
  isretr-eq-Eq-type-polynomial-endofunctor x y =
    isretr-map-inv-is-equiv (is-equiv-Eq-type-polynomial-endofunctor-eq x y)

  coh-refl-eq-Eq-type-polynomial-endofunctor :
    (x : type-polynomial-endofunctor A B X) →
    Id ( eq-Eq-type-polynomial-endofunctor x x
       ( refl-Eq-type-polynomial-endofunctor x)) refl
  coh-refl-eq-Eq-type-polynomial-endofunctor x =
    isretr-eq-Eq-type-polynomial-endofunctor x x refl
```

### The action on morphisms of the polynomial endofunctor

```agda
map-polynomial-endofunctor :
  {l1 l2 l3 l4 : Level} (A : UU l1) (B : A → UU l2) {X : UU l3} {Y : UU l4}
  (f : X → Y) →
  type-polynomial-endofunctor A B X → type-polynomial-endofunctor A B Y
map-polynomial-endofunctor A B f = tot (λ x α → f ∘ α)
```

### The action on homotopies of the polynomial endofunctor

```agda
htpy-polynomial-endofunctor :
  {l1 l2 l3 l4 : Level} (A : UU l1) (B : A → UU l2) {X : UU l3} {Y : UU l4}
  {f g : X → Y} →
  f ~ g → map-polynomial-endofunctor A B f ~ map-polynomial-endofunctor A B g
htpy-polynomial-endofunctor A B {X = X} {Y} {f} {g} H (pair x α) =
  eq-Eq-type-polynomial-endofunctor
    ( map-polynomial-endofunctor A B f (pair x α))
    ( map-polynomial-endofunctor A B g (pair x α))
    ( pair refl (H ·r α))

coh-refl-htpy-polynomial-endofunctor :
  {l1 l2 l3 l4 : Level} (A : UU l1) (B : A → UU l2) {X : UU l3} {Y : UU l4}
  (f : X → Y) →
  htpy-polynomial-endofunctor A B (refl-htpy {f = f}) ~ refl-htpy
coh-refl-htpy-polynomial-endofunctor A B {X = X} {Y} f (pair x α) =
  coh-refl-eq-Eq-type-polynomial-endofunctor
    ( map-polynomial-endofunctor A B f (pair x α))
```
