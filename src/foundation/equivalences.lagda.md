# Equivalences

```agda
{-# OPTIONS --without-K --exact-split #-}

module foundation.equivalences where

open import foundation-core.equivalences public

open import foundation-core.coherently-invertible-maps using
  ( is-coherently-invertible)
open import foundation-core.commuting-squares using (coherence-square)
open import foundation-core.contractible-maps using
  ( is-contr-map-is-equiv; is-contr-map)
open import foundation-core.contractible-types using (center; eq-is-contr')
open import foundation-core.dependent-pair-types using (Σ; pair; pr1; pr2)
open import foundation-core.embeddings using (is-emb; _↪_)
open import foundation-core.fibers-of-maps using (fib)
open import foundation-core.functions using (_∘_; id; precomp-Π; precomp)
open import foundation-core.functoriality-dependent-pair-types using
  ( tot; equiv-tot; is-equiv-tot-is-fiberwise-equiv)
open import foundation-core.fundamental-theorem-of-identity-types using
  ( fundamental-theorem-id; fundamental-theorem-id')
open import foundation-core.homotopies using (_~_; refl-htpy)
open import foundation-core.path-split-maps using
  ( is-coherently-invertible-is-path-split; is-path-split-is-equiv)
open import foundation-core.propositions using
  ( UU-Prop; type-Prop; is-prop-type-Prop; is-prop)
open import foundation-core.retractions using (retr)
open import foundation-core.sections using (sec)
open import foundation-core.sets using (UU-Set; type-Set; is-set)
open import foundation-core.truncated-types using
  ( UU-Truncated-Type; type-Truncated-Type; is-trunc)
open import foundation-core.truncation-levels using (𝕋)
open import foundation-core.universe-levels using (Level; UU; _⊔_)

open import foundation.contractible-types using
  ( is-contr; is-contr-equiv; is-contr-equiv'; is-contr-Π; is-contr-is-equiv';
    is-contr-prod; is-prop-is-contr)
open import
  foundation.distributivity-of-dependent-functions-over-dependent-pairs using
  ( distributive-Π-Σ)
open import foundation.function-extensionality using
  ( htpy-eq; funext; eq-htpy; equiv-funext)
open import foundation.identity-systems using (Ind-identity-system)
open import foundation.identity-types using
  ( Id; refl; equiv-inv; ap; equiv-concat'; inv; _∙_; concat'; assoc; concat;
    left-inv; right-unit; distributive-inv-concat; con-inv; inv-inv; ap-inv;
    ap-concat; ap-binary; inv-con; ap-comp; ap-id; tr; apd)
open import foundation.subtype-identity-principle using
  ( extensionality-subtype)
open import foundation.subtypes using (is-emb-pr1; equiv-subtype-equiv)
```

## Properties

### Any equivalence is an embedding

We already proved in `foundation-core.equivalences` that equivalences are embeddings. Here we have `_↪_` available, so we record the map from equivalences to embeddings.

```agda
module _
  {l1 l2 : Level} {A : UU l1} {B : UU l2}
  where
  
  emb-equiv : (A ≃ B) → (A ↪ B)
  pr1 (emb-equiv e) = map-equiv e
  pr2 (emb-equiv e) = is-emb-is-equiv (is-equiv-map-equiv e)
```

### Transposing equalities along equivalences

The fact that equivalences are embeddings has many important consequences, we will use some of these consequences in order to derive basic properties of embeddings.

```agda
module _
  {l1 l2 : Level} {A : UU l1} {B : UU l2} (e : A ≃ B)
  where

  eq-transpose-equiv :
    (x : A) (y : B) → (Id (map-equiv e x) y) ≃ (Id x (map-inv-equiv e y))
  eq-transpose-equiv x y =
    ( inv-equiv (equiv-ap e x (map-inv-equiv e y))) ∘e
    ( equiv-concat'
      ( map-equiv e x)
      ( inv (issec-map-inv-equiv e y)))

  map-eq-transpose-equiv :
    {x : A} {y : B} → Id (map-equiv e x) y → Id x (map-inv-equiv e y)
  map-eq-transpose-equiv {x} {y} = map-equiv (eq-transpose-equiv x y)

  inv-map-eq-transpose-equiv :
    {x : A} {y : B} → Id x (map-inv-equiv e y) → Id (map-equiv e x) y
  inv-map-eq-transpose-equiv {x} {y} = map-inv-equiv (eq-transpose-equiv x y)

  triangle-eq-transpose-equiv :
    {x : A} {y : B} (p : Id (map-equiv e x) y) →
    Id ( ( ap (map-equiv e) (map-eq-transpose-equiv p)) ∙
         ( issec-map-inv-equiv e y))
       ( p)
  triangle-eq-transpose-equiv {x} {y} p =
    ( ap ( concat' (map-equiv e x) (issec-map-inv-equiv e y))
         ( issec-map-inv-equiv
           ( equiv-ap e x (map-inv-equiv e y))
           ( p ∙ inv (issec-map-inv-equiv e y)))) ∙
    ( ( assoc p (inv (issec-map-inv-equiv e y)) (issec-map-inv-equiv e y)) ∙
      ( ( ap (concat p y) (left-inv (issec-map-inv-equiv e y))) ∙ right-unit))
  
  map-eq-transpose-equiv' :
    {a : A} {b : B} → Id b (map-equiv e a) → Id (map-inv-equiv e b) a
  map-eq-transpose-equiv' p = inv (map-eq-transpose-equiv (inv p))

  inv-map-eq-transpose-equiv' :
    {a : A} {b : B} → Id (map-inv-equiv e b) a → Id b (map-equiv e a)
  inv-map-eq-transpose-equiv' p =
    inv (inv-map-eq-transpose-equiv (inv p))

  triangle-eq-transpose-equiv' :
    {x : A} {y : B} (p : Id y (map-equiv e x)) →
    Id ( (issec-map-inv-equiv e y) ∙ p)
      ( ap (map-equiv e) (map-eq-transpose-equiv' p))
  triangle-eq-transpose-equiv' {x} {y} p =
    map-inv-equiv
      ( equiv-ap
        ( equiv-inv (map-equiv e (map-inv-equiv e y)) (map-equiv e x))
        ( (issec-map-inv-equiv e y) ∙ p)
        ( ap (map-equiv e) (map-eq-transpose-equiv' p)))
      ( ( distributive-inv-concat (issec-map-inv-equiv e y) p) ∙
        ( ( inv
            ( con-inv
              ( ap (map-equiv e) (inv (map-eq-transpose-equiv' p)))
              ( issec-map-inv-equiv e y)
              ( inv p)
              ( ( ap ( concat' (map-equiv e x) (issec-map-inv-equiv e y))
                     ( ap ( ap (map-equiv e))
                          ( inv-inv
                            ( map-inv-equiv
                              ( equiv-ap e x (map-inv-equiv e y))
                              ( ( inv p) ∙
                                ( inv (issec-map-inv-equiv e y))))))) ∙
                ( triangle-eq-transpose-equiv (inv p))))) ∙
          ( ap-inv (map-equiv e) (map-eq-transpose-equiv' p))))
```

### If `f` is coherently invertible, then precomposing by `f` is an equivalence

```agda
tr-precompose-fam :
  {l1 l2 l3 : Level} {A : UU l1} {B : UU l2} (C : B → UU l3)
  (f : A → B) {x y : A} (p : Id x y) → tr C (ap f p) ~ tr (λ x → C (f x)) p
tr-precompose-fam C f refl = refl-htpy

abstract
  is-equiv-precomp-Π-is-coherently-invertible :
    {l1 l2 l3 : Level} {A : UU l1} {B : UU l2} (f : A → B) →
    is-coherently-invertible f →
    (C : B → UU l3) → is-equiv (precomp-Π f C)
  is-equiv-precomp-Π-is-coherently-invertible f
    ( pair g (pair issec-g (pair isretr-g coh))) C = 
    is-equiv-has-inverse
      (λ s y → tr C (issec-g y) (s (g y)))
      ( λ s → eq-htpy (λ x → 
        ( ap (λ t → tr C t (s (g (f x)))) (coh x)) ∙
        ( ( tr-precompose-fam C f (isretr-g x) (s (g (f x)))) ∙
          ( apd s (isretr-g x)))))
      ( λ s → eq-htpy λ y → apd s (issec-g y))
```

### If `f` is an equivalence, then precomposing by `f` is an equivalence

```agda
abstract
  is-equiv-precomp-Π-is-equiv :
    {l1 l2 l3 : Level} {A : UU l1} {B : UU l2} (f : A → B) → is-equiv f →
    (C : B → UU l3) → is-equiv (precomp-Π f C)
  is-equiv-precomp-Π-is-equiv f is-equiv-f =
    is-equiv-precomp-Π-is-coherently-invertible f
      ( is-coherently-invertible-is-path-split f
        ( is-path-split-is-equiv f is-equiv-f))

equiv-precomp-Π :
  {l1 l2 l3 : Level} {A : UU l1} {B : UU l2} (e : A ≃ B) →
  (C : B → UU l3) → ((b : B) → C b) ≃ ((a : A) → C (map-equiv e a))
pr1 (equiv-precomp-Π e C) = precomp-Π (map-equiv e) C
pr2 (equiv-precomp-Π e C) =
  is-equiv-precomp-Π-is-equiv (map-equiv e) (is-equiv-map-equiv e) C
```

### Equivalences can be seen as constructors for inductive types.

```agda
abstract
  ind-is-equiv :
    {l1 l2 l3 : Level} {A : UU l1} {B : UU l2}
    (C : B → UU l3) (f : A → B) (is-equiv-f : is-equiv f) →
    ((x : A) → C (f x)) → ((y : B) → C y)
  ind-is-equiv C f is-equiv-f =
    map-inv-is-equiv (is-equiv-precomp-Π-is-equiv f is-equiv-f C)
  
  comp-is-equiv :
    {l1 l2 l3 : Level} {A : UU l1} {B : UU l2} (C : B → UU l3)
    (f : A → B) (is-equiv-f : is-equiv f) (h : (x : A) → C (f x)) →
    Id (λ x → (ind-is-equiv C f is-equiv-f h) (f x)) h
  comp-is-equiv C f is-equiv-f h =
    issec-map-inv-is-equiv (is-equiv-precomp-Π-is-equiv f is-equiv-f C) h
  
  htpy-comp-is-equiv :
    {l1 l2 l3 : Level} {A : UU l1} {B : UU l2}
    (C : B → UU l3) (f : A → B) (is-equiv-f : is-equiv f)
    (h : (x : A) → C (f x)) →
    (λ x → (ind-is-equiv C f is-equiv-f h) (f x)) ~ h
  htpy-comp-is-equiv C f is-equiv-f h = htpy-eq (comp-is-equiv C f is-equiv-f h)
```

## If dependent precomposition by `f` is an equivalence, then precomposition by `f` is an equivalence

```agda
abstract
  is-equiv-precomp-is-equiv-precomp-Π :
    {l1 l2 l3 : Level} {A : UU l1} {B : UU l2} (f : A → B) →
    ((C : B → UU l3) → is-equiv (precomp-Π f C)) →
    ((C : UU l3) → is-equiv (precomp f C))
  is-equiv-precomp-is-equiv-precomp-Π f is-equiv-precomp-Π-f C =
    is-equiv-precomp-Π-f (λ y → C)
```

### If `f` is an equivalence, then precomposition by `f` is an equivalence

```agda
abstract
  is-equiv-precomp-is-equiv :
    {l1 l2 l3 : Level} {A : UU l1} {B : UU l2} (f : A → B) → is-equiv f →
    (C : UU l3) → is-equiv (precomp f C)
  is-equiv-precomp-is-equiv f is-equiv-f =
    is-equiv-precomp-is-equiv-precomp-Π f
      ( is-equiv-precomp-Π-is-equiv f is-equiv-f)

equiv-precomp :
  {l1 l2 l3 : Level} {A : UU l1} {B : UU l2} (e : A ≃ B) (C : UU l3) →
  (B → C) ≃ (A → C)
pr1 (equiv-precomp e C) = precomp (map-equiv e) C
pr2 (equiv-precomp e C) =
  is-equiv-precomp-is-equiv (map-equiv e) (is-equiv-map-equiv e) C
```

### If precomposing by `f` is an equivalence, then `f` is an equivalence

First, we prove this relative to a subuniverse, such that `f` is a map between two types in that subuniverse.

```agda
abstract
  is-equiv-is-equiv-precomp-subuniverse :
    { l1 l2 : Level}
    ( α : Level → Level) (P : (l : Level) → UU l → UU (α l))
    ( A : Σ (UU l1) (P l1)) (B : Σ (UU l2) (P l2)) (f : pr1 A → pr1 B) →
    ( (l : Level) (C : Σ (UU l) (P l)) →
      is-equiv (precomp f (pr1 C))) →
    is-equiv f
  is-equiv-is-equiv-precomp-subuniverse α P A B f is-equiv-precomp-f =
    let retr-f = center (is-contr-map-is-equiv (is-equiv-precomp-f _ A) id) in
    is-equiv-has-inverse
      ( pr1 retr-f)
      ( htpy-eq
        ( ap ( pr1)
             ( eq-is-contr'
               ( is-contr-map-is-equiv (is-equiv-precomp-f _ B) f)
                 ( pair
                   ( f ∘ (pr1 retr-f))
                   ( ap (λ (g : pr1 A → pr1 A) → f ∘ g) (pr2 retr-f)))
                 ( pair id refl))))
      ( htpy-eq (pr2 retr-f))
```

Now we prove the usual statement, without the subuniverse

```agda
module _
  {l1 l2 : Level} {A : UU l1} {B : UU l2}
  where
  
  abstract
    is-equiv-is-equiv-precomp :
      (f : A → B) → ((l : Level) (C : UU l) → is-equiv (precomp f C)) →
      is-equiv f
    is-equiv-is-equiv-precomp f is-equiv-precomp-f =
      is-equiv-is-equiv-precomp-subuniverse
        ( λ l → l1 ⊔ l2)
        ( λ l X → A → B)
        ( pair A f)
        ( pair B f)
        ( f)
        ( λ l C → is-equiv-precomp-f l (pr1 C))
```

```agda
is-equiv-is-equiv-precomp-Prop :
  {l1 l2 : Level} (P : UU-Prop l1) (Q : UU-Prop l2)
  (f : type-Prop P → type-Prop Q) →
  ({l : Level} (R : UU-Prop l) → is-equiv (precomp f (type-Prop R))) →
  is-equiv f
is-equiv-is-equiv-precomp-Prop P Q f H =
  is-equiv-is-equiv-precomp-subuniverse id (λ l → is-prop) P Q f (λ l → H {l})

is-equiv-is-equiv-precomp-Set :
  {l1 l2 : Level} (A : UU-Set l1) (B : UU-Set l2)
  (f : type-Set A → type-Set B) →
  ({l : Level} (C : UU-Set l) → is-equiv (precomp f (type-Set C))) →
  is-equiv f
is-equiv-is-equiv-precomp-Set A B f H =
  is-equiv-is-equiv-precomp-subuniverse id (λ l → is-set) A B f (λ l → H {l})

is-equiv-is-equiv-precomp-Truncated-Type :
  {l1 l2 : Level} (k : 𝕋)
  (A : UU-Truncated-Type l1 k) (B : UU-Truncated-Type l2 k)
  (f : type-Truncated-Type A → type-Truncated-Type B) →
  ({l : Level} (C : UU-Truncated-Type l k) → is-equiv (precomp f (pr1 C))) →
  is-equiv f
is-equiv-is-equiv-precomp-Truncated-Type k A B f H =
    is-equiv-is-equiv-precomp-subuniverse id (λ l → is-trunc k) A B f
      ( λ l → H {l})
```

### Equivalences have a contractible type of sections

```agda
module _
  {l1 l2 : Level} {A : UU l1} {B : UU l2}
  where

  is-contr-sec-is-equiv : {f : A → B} → is-equiv f → is-contr (sec f)
  is-contr-sec-is-equiv {f} is-equiv-f =
    is-contr-equiv'
      ( (b : B) → fib f b)
      ( distributive-Π-Σ) 
      ( is-contr-Π (is-contr-map-is-equiv is-equiv-f))
```

### Equivalences have a contractible type of retractions

```agda
module _
  {l1 l2 : Level} {A : UU l1} {B : UU l2}
  where

  is-contr-retr-is-equiv : {f : A → B} → is-equiv f → is-contr (retr f)
  is-contr-retr-is-equiv {f} is-equiv-f =
    is-contr-is-equiv'
      ( Σ (B → A) (λ h → Id (h ∘ f) id))
      ( tot (λ h → htpy-eq))
      ( is-equiv-tot-is-fiberwise-equiv
        ( λ h → funext (h ∘ f) id))
      ( is-contr-map-is-equiv (is-equiv-precomp-is-equiv f is-equiv-f A) id)
```

### Being an equivalence is a property

```agda
module _
  {l1 l2 : Level} {A : UU l1} {B : UU l2}
  where

  is-contr-is-equiv-is-equiv : {f : A → B} → is-equiv f → is-contr (is-equiv f)
  is-contr-is-equiv-is-equiv is-equiv-f =
    is-contr-prod
      ( is-contr-sec-is-equiv is-equiv-f)
      ( is-contr-retr-is-equiv is-equiv-f)

  abstract
    is-property-is-equiv : (f : A → B) → (H K : is-equiv f) → is-contr (Id H K)
    is-property-is-equiv f H =
      is-prop-is-contr (is-contr-is-equiv-is-equiv H) H

  is-equiv-Prop :
    (f : A → B) → Σ (UU (l1 ⊔ l2)) (λ X → (x y : X) → is-contr (Id x y))
  pr1 (is-equiv-Prop f) = is-equiv f
  pr2 (is-equiv-Prop f) = is-property-is-equiv f

  abstract
    is-emb-map-equiv :
      is-emb (map-equiv {A = A} {B = B})
    is-emb-map-equiv = is-emb-pr1 is-property-is-equiv
```

### Characterizing the identity type of equivalences

```agda
module _
  {l1 l2 : Level} {A : UU l1} {B : UU l2}
  where

  htpy-equiv : A ≃ B → A ≃ B → UU (l1 ⊔ l2)
  htpy-equiv e e' = (map-equiv e) ~ (map-equiv e')

  extensionality-equiv : (f g : A ≃ B) → Id f g ≃ htpy-equiv f g
  extensionality-equiv f =
    extensionality-subtype
      ( is-equiv-Prop)
      ( pr2 f)
      ( refl-htpy {f = pr1 f})
      ( λ g → equiv-funext)
  
  refl-htpy-equiv : (e : A ≃ B) → htpy-equiv e e
  refl-htpy-equiv e = refl-htpy

  abstract
    is-contr-total-htpy-equiv :
      (e : A ≃ B) → is-contr (Σ (A ≃ B) (htpy-equiv e))
    is-contr-total-htpy-equiv e =
      fundamental-theorem-id' e
        ( refl-htpy-equiv e)
        ( λ f → map-equiv (extensionality-equiv e f))
        ( λ f → is-equiv-map-equiv (extensionality-equiv e f))

  eq-htpy-equiv : {e e' : A ≃ B} → (htpy-equiv e e') → Id e e'
  eq-htpy-equiv {e = e} {e'} = map-inv-equiv (extensionality-equiv e e')

  htpy-eq-equiv : {e e' : A ≃ B} → Id e e' → htpy-equiv e e'
  htpy-eq-equiv {e} {e'} = map-equiv (extensionality-equiv e e')
```

### Homotopy induction for homotopies between equivalences

```agda
module _
  {l1 l2 : Level} {A : UU l1} {B : UU l2}
  where
  
  abstract
    Ind-htpy-equiv :
      {l3 : Level} (e : A ≃ B)
      (P : (e' : A ≃ B) (H : htpy-equiv e e') → UU l3) →
      sec
        ( λ (h : (e' : A ≃ B) (H : htpy-equiv e e') → P e' H) →
          h e (refl-htpy-equiv e))
    Ind-htpy-equiv e =
      Ind-identity-system e
        ( refl-htpy-equiv e)
        ( is-contr-total-htpy-equiv e)
  
  ind-htpy-equiv :
    {l3 : Level} (e : A ≃ B) (P : (e' : A ≃ B) (H : htpy-equiv e e') → UU l3) →
    P e (refl-htpy-equiv e) → (e' : A ≃ B) (H : htpy-equiv e e') → P e' H
  ind-htpy-equiv e P = pr1 (Ind-htpy-equiv e P)
  
  comp-htpy-equiv :
    {l3 : Level} (e : A ≃ B) (P : (e' : A ≃ B) (H : htpy-equiv e e') → UU l3)
    (p : P e (refl-htpy-equiv e)) →
    Id (ind-htpy-equiv e P p e (refl-htpy-equiv e)) p
  comp-htpy-equiv e P = pr2 (Ind-htpy-equiv e P)
```

### The groupoid laws for equivalences

```agda
associative-comp-equiv :
  {l1 l2 l3 l4 : Level} {A : UU l1} {B : UU l2} {C : UU l3} {D : UU l4} →
  (e : A ≃ B) (f : B ≃ C) (g : C ≃ D) →
  Id ((g ∘e f) ∘e e) (g ∘e (f ∘e e))
associative-comp-equiv e f g = eq-htpy-equiv refl-htpy

module _
  {l1 l2 : Level} {X : UU l1} {Y : UU l2}
  where

  left-unit-law-equiv : (e : X ≃ Y) → Id (id-equiv ∘e e) e
  left-unit-law-equiv e = eq-htpy-equiv refl-htpy
  
  right-unit-law-equiv : (e : X ≃ Y) → Id (e ∘e id-equiv) e
  right-unit-law-equiv e = eq-htpy-equiv refl-htpy
  
  left-inverse-law-equiv : (e : X ≃ Y) → Id ((inv-equiv e) ∘e e) id-equiv
  left-inverse-law-equiv e =
    eq-htpy-equiv (isretr-map-inv-is-equiv (is-equiv-map-equiv e))
  
  right-inverse-law-equiv : (e : X ≃ Y) → Id (e ∘e (inv-equiv e)) id-equiv
  right-inverse-law-equiv e =
    eq-htpy-equiv (issec-map-inv-is-equiv (is-equiv-map-equiv e))

  inv-inv-equiv : (e : X ≃ Y) → Id (inv-equiv (inv-equiv e)) e
  inv-inv-equiv e = eq-htpy-equiv refl-htpy

  inv-inv-equiv' : (e : Y ≃ X) → Id (inv-equiv (inv-equiv e)) e
  inv-inv-equiv' e = eq-htpy-equiv refl-htpy

  is-equiv-inv-equiv : is-equiv (inv-equiv {A = X} {B = Y})
  is-equiv-inv-equiv =
    is-equiv-has-inverse
      ( inv-equiv)
      ( inv-inv-equiv')
      ( inv-inv-equiv)

  equiv-inv-equiv : (X ≃ Y) ≃ (Y ≃ X)
  pr1 equiv-inv-equiv = inv-equiv
  pr2 equiv-inv-equiv = is-equiv-inv-equiv

compose-inv-equiv-compose-equiv :
  {l1 l2 l3 : Level} {A : UU l1} {B : UU l2} {C : UU l3}
  (f : B ≃ C) (e : A ≃ B) →
  Id (inv-equiv f ∘e (f ∘e e)) e
compose-inv-equiv-compose-equiv f e =
  eq-htpy-equiv (λ x → isretr-map-inv-equiv f (map-equiv e x))

compose-equiv-compose-inv-equiv :
  {l1 l2 l3 : Level} {A : UU l1} {B : UU l2} {C : UU l3}
  (f : B ≃ C) (e : A ≃ C) →
  Id (f ∘e (inv-equiv f ∘e e)) e
compose-equiv-compose-inv-equiv f e =
  eq-htpy-equiv (λ x → issec-map-inv-equiv f (map-equiv e x))

is-equiv-comp-equiv :
  {l1 l2 l3 : Level} {B : UU l2} {C : UU l3}
  (f : B ≃ C) (A : UU l1) → is-equiv (λ (e : A ≃ B) → f ∘e e)
is-equiv-comp-equiv f A =
  is-equiv-has-inverse
    ( λ e → inv-equiv f ∘e e)
    ( compose-equiv-compose-inv-equiv f)
    ( compose-inv-equiv-compose-equiv f)

equiv-postcomp-equiv :
  {l1 l2 l3 : Level} {B : UU l2} {C : UU l3} →
  (f : B ≃ C) → (A : UU l1) → (A ≃ B) ≃ (A ≃ C)
pr1 (equiv-postcomp-equiv f A) e = f ∘e e
pr2 (equiv-postcomp-equiv f A) = is-equiv-comp-equiv f A
```

```agda
equiv-precomp-equiv :
  {l1 l2 l3 : Level} {A : UU l1} {B : UU l2} →
  (A ≃ B) → (C : UU l3) → (B ≃ C) ≃ (A ≃ C)
equiv-precomp-equiv e C =
  equiv-subtype-equiv
    ( equiv-precomp e C)
    ( is-equiv-Prop)
    ( is-equiv-Prop)
    ( λ g →
      pair
        ( is-equiv-comp' g (map-equiv e) (is-equiv-map-equiv e))
        ( λ is-equiv-eg →
          is-equiv-left-factor'
            g (map-equiv e) is-equiv-eg (is-equiv-map-equiv e)))
```
