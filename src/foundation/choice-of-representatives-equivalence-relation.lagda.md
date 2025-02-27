# Choice of representatives for an equivalence relation

```agda
{-# OPTIONS --without-K --exact-split #-}

module foundation.choice-of-representatives-equivalence-relation where

open import foundation.cartesian-product-types using (_×_)
open import foundation.contractible-types using
  ( is-contr; center; is-contr-equiv)
open import foundation.dependent-pair-types using (Σ; pair; pr1; pr2)
open import foundation.embeddings using (is-emb)
open import foundation.equivalence-classes using
  ( large-set-quotient; quotient-map-large-set-quotient;
    apply-effectiveness-quotient-map-large-set-quotient';
    is-effective-quotient-map-large-set-quotient)
open import foundation.equivalence-relations using
  ( Eq-Rel; type-Eq-Rel; symm-Eq-Rel; prop-Eq-Rel)
open import foundation.equivalences using (_∘e_; is-equiv; _≃_)
open import foundation.fibers-of-maps using (fib)
open import foundation.functoriality-dependent-pair-types using (equiv-tot)
open import foundation.fundamental-theorem-of-identity-types using
  ( fundamental-theorem-id)
open import foundation.identity-types using (_∙_; ap; refl)
open import foundation.propositional-truncations using
  ( apply-universal-property-trunc-Prop; trunc-Prop; unit-trunc-Prop;
    is-prop-type-trunc-Prop)
open import foundation.propositions using (eq-is-prop)
open import foundation.surjective-maps using
  ( is-surjective; is-equiv-is-emb-is-surjective)
open import foundation.type-arithmetic-dependent-pair-types using (assoc-Σ)
open import foundation.universe-levels using (Level; UU; _⊔_)
```

## Idea

If we can construct a choice of representatives for each equivalence class, then we can construct the set quotient as a retract of the original type.

```agda
module _
  {l1 l2 l3 : Level} {A : UU l1} (R : Eq-Rel l2 A)
  where
    
  is-choice-of-representatives :
    (A → UU l3) → UU (l1 ⊔ l2 ⊔ l3)
  is-choice-of-representatives P =
    (a : A) → is-contr (Σ A (λ x → P x × type-Eq-Rel R a x))
  
  representatives :
    {P : A → UU l3} → is-choice-of-representatives P → UU (l1 ⊔ l3)
  representatives {P} H = Σ A P
  
  quotient-map-large-set-quotient-representatives :
    {P : A → UU l3} (H : is-choice-of-representatives P) →
    representatives H → large-set-quotient R
  quotient-map-large-set-quotient-representatives H a =
    quotient-map-large-set-quotient R (pr1 a)

  abstract
    is-surjective-quotient-map-large-set-quotient-representatives :
      {P : A → UU l3} (H : is-choice-of-representatives P) →
      is-surjective (quotient-map-large-set-quotient-representatives H)
    is-surjective-quotient-map-large-set-quotient-representatives H (pair Q K) =
      apply-universal-property-trunc-Prop K
        ( trunc-Prop
          ( fib (quotient-map-large-set-quotient-representatives H) (pair Q K)))
        ( λ { (pair a refl) →
              unit-trunc-Prop
                ( pair
                  ( pair (pr1 (center (H a))) (pr1 (pr2 (center (H a)))))
                  ( ( apply-effectiveness-quotient-map-large-set-quotient' R
                      ( symm-Eq-Rel R (pr2 (pr2 (center (H a)))))) ∙
                    ( ap
                      ( pair (prop-Eq-Rel R a))
                      ( eq-is-prop is-prop-type-trunc-Prop))))})

  abstract
    is-emb-quotient-map-large-set-quotient-representatives :
      {P : A → UU l3} (H : is-choice-of-representatives P) →
      is-emb (quotient-map-large-set-quotient-representatives H)
    is-emb-quotient-map-large-set-quotient-representatives {P} H (pair a p) =
      fundamental-theorem-id
        ( pair a p)
        ( refl)
        ( is-contr-equiv
          ( Σ A (λ x → P x × type-Eq-Rel R a x))
          ( ( assoc-Σ A P (λ z → type-Eq-Rel R a (pr1 z))) ∘e
            ( equiv-tot
              ( λ t →
                is-effective-quotient-map-large-set-quotient R a (pr1 t))))
          ( H a))
        ( λ y →
          ap (quotient-map-large-set-quotient-representatives H) {pair a p} {y})

  abstract
    is-equiv-quotient-map-large-set-quotient-representatives :
      {P : A → UU l3} (H : is-choice-of-representatives P) →
      is-equiv (quotient-map-large-set-quotient-representatives H)
    is-equiv-quotient-map-large-set-quotient-representatives H =
      is-equiv-is-emb-is-surjective
        ( is-surjective-quotient-map-large-set-quotient-representatives H)
        ( is-emb-quotient-map-large-set-quotient-representatives H)

  equiv-large-set-quotient-representatives :
    {P : A → UU l3} (H : is-choice-of-representatives P) →
    representatives H ≃ large-set-quotient R
  equiv-large-set-quotient-representatives H =
    pair ( quotient-map-large-set-quotient-representatives H)
         ( is-equiv-quotient-map-large-set-quotient-representatives H)
```
