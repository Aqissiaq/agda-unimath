# Functoriality of propositional truncations

```agda
{-# OPTIONS --without-K --exact-split #-}

module foundation.functoriality-propositional-truncation where

open import foundation.contractible-types using
  ( is-contr; center; contraction)
open import foundation.dependent-pair-types using (Σ; pair; pr1; pr2)
open import foundation.equivalences using (_≃_; map-equiv; inv-equiv)
open import foundation.function-extensionality using (htpy-eq)
open import foundation.functions using (_∘_; id)
open import foundation.homotopies using (_~_; refl-htpy; _·l_; _∙h_; _·r_)
open import foundation.identity-types using (ap)
open import foundation.propositional-truncations using
  ( trunc-Prop; unit-trunc-Prop; universal-property-trunc-Prop; type-trunc-Prop;
    is-prop-type-trunc-Prop)
open import foundation.propositions using (type-hom-Prop; is-equiv-is-prop)
open import foundation.universe-levels using (Level; UU)
```

## Idea

The universal property of propositional truncations can be used to define the functorial action of propositional truncations.

## Definition

```agda
abstract
  unique-functor-trunc-Prop :
    {l1 l2 : Level} {A : UU l1} {B : UU l2} (f : A → B) →
    is-contr
      ( Σ ( type-hom-Prop (trunc-Prop A) (trunc-Prop B))
          ( λ h → (h ∘ unit-trunc-Prop) ~ (unit-trunc-Prop ∘ f)))
  unique-functor-trunc-Prop {l1} {l2} {A} {B} f =
    universal-property-trunc-Prop A (trunc-Prop B) (unit-trunc-Prop ∘ f)

abstract
  functor-trunc-Prop :
    {l1 l2 : Level} {A : UU l1} {B : UU l2} →
    (A → B) → type-hom-Prop (trunc-Prop A) (trunc-Prop B)
  functor-trunc-Prop f =
    pr1 (center (unique-functor-trunc-Prop f))
```

## Properties

### Propositional truncations of homotopic maps are homotopic

```agda
  htpy-functor-trunc-Prop :
    { l1 l2 : Level} {A : UU l1} {B : UU l2} (f : A → B) →
    ( (functor-trunc-Prop f) ∘ unit-trunc-Prop) ~ (unit-trunc-Prop ∘ f)
  htpy-functor-trunc-Prop f =
    pr2 (center (unique-functor-trunc-Prop f))

  htpy-uniqueness-functor-trunc-Prop :
    { l1 l2 : Level} {A : UU l1} {B : UU l2} (f : A → B) →
    ( h : type-hom-Prop (trunc-Prop A) (trunc-Prop B)) →
    ( ( h ∘ unit-trunc-Prop) ~ (unit-trunc-Prop ∘ f)) →
    (functor-trunc-Prop f) ~ h
  htpy-uniqueness-functor-trunc-Prop f h H =
    htpy-eq (ap pr1 (contraction (unique-functor-trunc-Prop f) (pair h H)))
```

### The propositional truncation of the identity map is the identity map

```agda
abstract
  id-functor-trunc-Prop :
    { l1 : Level} {A : UU l1} → functor-trunc-Prop (id {A = A}) ~ id
  id-functor-trunc-Prop {l1} {A} =
    htpy-uniqueness-functor-trunc-Prop id id refl-htpy
```

### The propositional truncation of a composite is the composite of the propositional truncations

```agda
abstract
  comp-functor-trunc-Prop :
    { l1 l2 l3 : Level} {A : UU l1} {B : UU l2} {C : UU l3}
    ( g : B → C) (f : A → B) →
    ( functor-trunc-Prop (g ∘ f)) ~
    ( (functor-trunc-Prop g) ∘ (functor-trunc-Prop f))
  comp-functor-trunc-Prop g f =
    htpy-uniqueness-functor-trunc-Prop
      ( g ∘ f)
      ( (functor-trunc-Prop g) ∘ (functor-trunc-Prop f))
      ( ( (functor-trunc-Prop g) ·l (htpy-functor-trunc-Prop f)) ∙h
        ( ( htpy-functor-trunc-Prop g) ·r f))
```

### Propositional truncations of equivalences are equivalences

```agda
abstract
  map-equiv-trunc-Prop :
    {l1 l2 : Level} {A : UU l1} {B : UU l2} →
    (A ≃ B) → type-trunc-Prop A → type-trunc-Prop B
  map-equiv-trunc-Prop e = functor-trunc-Prop (map-equiv e)

abstract
  map-inv-equiv-trunc-Prop :
    {l1 l2 : Level} {A : UU l1} {B : UU l2} →
    (A ≃ B) → type-trunc-Prop B → type-trunc-Prop A
  map-inv-equiv-trunc-Prop e = map-equiv-trunc-Prop (inv-equiv e)

abstract
  equiv-trunc-Prop :
    {l1 l2 : Level} {A : UU l1} {B : UU l2} →
    (A ≃ B) → (type-trunc-Prop A ≃ type-trunc-Prop B)
  pr1 (equiv-trunc-Prop e) = map-equiv-trunc-Prop e
  pr2 (equiv-trunc-Prop e) =
    is-equiv-is-prop
      ( is-prop-type-trunc-Prop)
      ( is-prop-type-trunc-Prop)
      ( map-inv-equiv-trunc-Prop e)
```
