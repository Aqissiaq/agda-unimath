# Univalent mathematics in Agda

Welcome to the website of the `agda-unimath` formalization project.

[![build](https://github.com/UniMath/agda-unimath/actions/workflows/ci.yaml/badge.svg?branch=master)](https://github.com/UniMath/agda-unimath/actions/workflows/ci.yaml)

```agda
{-# OPTIONS --without-K --exact-split #-}
```

## Categories

```agda
open import categories
open import categories.adjunctions
open import categories.categories
open import categories.functors
open import categories.large-categories
open import categories.natural-transformations
```

## Elementary number theory

```agda
open import elementary-number-theory.absolute-value-integers
open import elementary-number-theory.addition-integers
open import elementary-number-theory.addition-natural-numbers
open import elementary-number-theory.binomial-coefficients
open import elementary-number-theory.classical-finite-types
open import elementary-number-theory.collatz-bijection
open import elementary-number-theory.collatz-conjecture
open import elementary-number-theory.congruence-integers
open import elementary-number-theory.congruence-natural-numbers
open import elementary-number-theory.decidable-dependent-function-types
open import elementary-number-theory.decidable-dependent-pair-types
open import elementary-number-theory.difference-integers
open import elementary-number-theory.distance-integers
open import elementary-number-theory.distance-natural-numbers
open import elementary-number-theory.divisibility-integers
open import elementary-number-theory.divisibility-natural-numbers
open import elementary-number-theory.divisibility-standard-finite-types
open import elementary-number-theory.equality-integers
open import elementary-number-theory.equality-natural-numbers
open import elementary-number-theory.euclidean-division-natural-numbers
open import elementary-number-theory.exponentiation-natural-numbers
open import elementary-number-theory.factorials
open import elementary-number-theory.falling-factorials
open import elementary-number-theory.fibonacci-sequence
open import elementary-number-theory.finitary-natural-numbers
open import elementary-number-theory.finitely-cyclic-maps
open import elementary-number-theory.fractions
open import elementary-number-theory.goldbach-conjecture
open import elementary-number-theory.greatest-common-divisor-integers
open import elementary-number-theory.greatest-common-divisor-natural-numbers
open import elementary-number-theory.inequality-integers
open import elementary-number-theory.inequality-natural-numbers
open import elementary-number-theory.inequality-standard-finite-types
open import elementary-number-theory.infinitude-of-primes
open import elementary-number-theory.integers
open import elementary-number-theory.iterating-functions
open import elementary-number-theory.lower-bounds-natural-numbers
open import elementary-number-theory.modular-arithmetic-standard-finite-types
open import elementary-number-theory.modular-arithmetic
open import elementary-number-theory.multiplication-integers
open import elementary-number-theory.multiplication-natural-numbers
open import elementary-number-theory.natural-numbers
open import elementary-number-theory.ordinal-induction-natural-numbers
open import elementary-number-theory.prime-numbers
open import elementary-number-theory.products-of-natural-numbers
open import elementary-number-theory.proper-divisors-natural-numbers
open import elementary-number-theory.rational-numbers
open import elementary-number-theory.relatively-prime-integers
open import elementary-number-theory.repeating-element-standard-finite-type
open import elementary-number-theory.retracts-of-natural-numbers
open import elementary-number-theory.retracts-of-standard-finite-types
open import elementary-number-theory.skipping-element-standard-finite-type
open import elementary-number-theory.stirling-numbers-of-the-second-kind
open import elementary-number-theory.strong-induction-natural-numbers
open import elementary-number-theory.sums-of-natural-numbers
open import elementary-number-theory.triangular-numbers
open import elementary-number-theory.twin-prime-conjecture
open import elementary-number-theory.universal-property-natural-numbers
open import elementary-number-theory.upper-bounds-natural-numbers
open import elementary-number-theory.well-ordering-principle-natural-numbers
open import elementary-number-theory.well-ordering-principle-standard-finite-types
```

## Finite groups

```agda
open import finite-groups.abstract-quaternion-group
open import finite-groups.finite-groups
open import finite-groups.concrete-quaternion-group
open import finite-groups.transpositions
```

## Foundation

```agda
open import foundation.0-maps
open import foundation.1-types
open import foundation.2-types
open import foundation.algebras-polynomial-endofunctors
open import foundation.automorphisms
open import foundation.axiom-of-choice
open import foundation.binary-relations
open import foundation.boolean-reflection
open import foundation.booleans
open import foundation.cantors-diagonal-argument
open import foundation.cartesian-product-types
open import foundation.choice-of-representatives-equivalence-relation
open import foundation.coherently-invertible-maps
open import foundation.commuting-squares
open import foundation.complements
open import foundation.conjunction
open import foundation.connected-components-universes
open import foundation.connected-components
open import foundation.connected-types
open import foundation.constant-maps
open import foundation.contractible-maps
open import foundation.contractible-types
open import foundation.coproduct-types
open import foundation.coslice
open import foundation.decidable-dependent-function-types
open import foundation.decidable-dependent-pair-types
open import foundation.decidable-embeddings
open import foundation.decidable-equality
open import foundation.decidable-maps
open import foundation.decidable-propositions
open import foundation.decidable-subtypes
open import foundation.decidable-types
open import foundation.dependent-pair-types
open import foundation.diagonal-maps-of-types
open import foundation.disjunction
open import foundation.distributivity-of-dependent-functions-over-coproduct-types
open import foundation.distributivity-of-dependent-functions-over-dependent-pairs
open import foundation.double-negation
open import foundation.effective-maps-equivalence-relations
open import foundation.elementhood-relation-w-types
open import foundation.embeddings
open import foundation.empty-types
open import foundation.epimorphisms-with-respect-to-sets
open import foundation.equality-cartesian-product-types
open import foundation.equality-coproduct-types
open import foundation.equality-dependent-function-types
open import foundation.equality-dependent-pair-types
open import foundation.equality-fibers-of-maps
open import foundation.equivalence-classes
open import foundation.equivalence-induction
open import foundation.equivalence-relations
open import foundation.equivalences-maybe
open import foundation.equivalences
open import foundation.existential-quantification
open import foundation.extensional-w-types
open import foundation.faithful-maps
open import foundation.fiber-inclusions
open import foundation.fibered-maps
open import foundation.fibers-of-maps
open import foundation.function-extensionality
open import foundation.functions
open import foundation.functoriality-cartesian-product-types
open import foundation.functoriality-coproduct-types
open import foundation.functoriality-dependent-function-types
open import foundation.functoriality-dependent-pair-types
open import foundation.functoriality-function-types
open import foundation.functoriality-propositional-truncation
open import foundation.functoriality-set-quotients
open import foundation.functoriality-set-truncation
open import foundation.fundamental-theorem-of-identity-types
open import foundation.global-choice
open import foundation.homotopies
open import foundation.identity-systems
open import foundation.identity-types
open import foundation.images
open import foundation.impredicative-encodings
open import foundation.indexed-w-types
open import foundation.induction-principle-propositional-truncation
open import foundation.induction-w-types
open import foundation.inequality-w-types
open import foundation.injective-maps
open import foundation.interchange-law
open import foundation.involutions
open import foundation.isolated-points
open import foundation.isomorphisms-of-sets
open import foundation.law-of-excluded-middle
open import foundation.lawveres-fixed-point-theorem
open import foundation.locally-small-types
open import foundation.logical-equivalences
open import foundation.maybe
open import foundation.mere-equality
open import foundation.mere-equivalences
open import foundation.monomorphisms
open import foundation.multisets
open import foundation.negation
open import foundation.non-contractible-types
open import foundation.path-split-maps
open import foundation.polynomial-endofunctors
open import foundation.propositional-extensionality
open import foundation.propositional-maps
open import foundation.propositional-truncations
open import foundation.propositions
open import foundation.pullbacks
open import foundation.raising-universe-levels
open import foundation.reflecting-maps-equivalence-relations
open import foundation.replacement
open import foundation.retractions
open import foundation.russells-paradox
open import foundation.sections
open import foundation.set-presented-types
open import foundation.set-truncations
open import foundation.sets
open import foundation.singleton-induction
open import foundation.slice
open import foundation.small-maps
open import foundation.small-multisets
open import foundation.small-types
open import foundation.small-universes
open import foundation.split-surjective-maps
open import foundation.structure-identity-principle
open import foundation.structure
open import foundation.subterminal-types
open import foundation.subtype-identity-principle
open import foundation.subtypes
open import foundation.subuniverses
open import foundation.surjective-maps
open import foundation.truncated-maps
open import foundation.truncated-types
open import foundation.truncation-levels
open import foundation.type-arithmetic-cartesian-product-types
open import foundation.type-arithmetic-coproduct-types
open import foundation.type-arithmetic-dependent-pair-types
open import foundation.type-arithmetic-empty-type
open import foundation.type-arithmetic-unit-type
open import foundation.uniqueness-image
open import foundation.uniqueness-set-quotients
open import foundation.uniqueness-set-truncations
open import foundation.uniqueness-truncation
open import foundation.unit-type
open import foundation.univalence-implies-function-extensionality
open import foundation.univalence
open import foundation.univalent-type-families
open import foundation.universal-property-booleans
open import foundation.universal-property-cartesian-product-types
open import foundation.universal-property-coproduct-types
open import foundation.universal-property-dependent-pair-types
open import foundation.universal-property-empty-type
open import foundation.universal-property-fiber-products
open import foundation.universal-property-identity-types
open import foundation.universal-property-image
open import foundation.universal-property-maybe
open import foundation.universal-property-propositional-truncation-into-sets
open import foundation.universal-property-propositional-truncation
open import foundation.universal-property-pullbacks
open import foundation.universal-property-set-quotients
open import foundation.universal-property-set-truncation
open import foundation.universal-property-truncation
open import foundation.universal-property-unit-type
open import foundation.universe-levels
open import foundation.unordered-pairs
open import foundation.w-types
open import foundation.weak-function-extensionality
open import foundation.weakly-constant-maps
```

## Foundation Core

```agda
open import foundation-core.0-maps
open import foundation-core.1-types
open import foundation-core.cartesian-product-types
open import foundation-core.coherently-invertible-maps
open import foundation-core.commuting-squares
open import foundation-core.constant-maps
open import foundation-core.contractible-maps
open import foundation-core.contractible-types
open import foundation-core.dependent-pair-types
open import foundation-core.embeddings
open import foundation-core.equality-cartesian-product-types
open import foundation-core.equality-dependent-pair-types
open import foundation-core.equality-fibers-of-maps
open import foundation-core.equivalence-induction
open import foundation-core.equivalences
open import foundation-core.faithful-maps
open import foundation-core.fibers-of-maps
open import foundation-core.functions
open import foundation-core.functoriality-dependent-pair-types
open import foundation-core.fundamental-theorem-of-identity-types
open import foundation-core.homotopies
open import foundation-core.identity-systems
open import foundation-core.identity-types
open import foundation-core.logical-equivalences
open import foundation-core.path-split-maps
open import foundation-core.propositional-maps
open import foundation-core.propositions
open import foundation-core.retractions
open import foundation-core.sections
open import foundation-core.sets
open import foundation-core.singleton-induction
open import foundation-core.subtype-identity-principle
open import foundation-core.subtypes
open import foundation-core.truncated-maps
open import foundation-core.truncated-types
open import foundation-core.truncation-levels
open import foundation-core.type-arithmetic-cartesian-product-types
open import foundation-core.type-arithmetic-dependent-pair-types
open import foundation-core.univalence
open import foundation-core.universe-levels
```

## Graph theory

```agda
open import graph-theory.directed-graphs
open import graph-theory.finite-graphs
open import graph-theory.polygons
open import graph-theory.reflexive-graphs
open import graph-theory.undirected-graphs
```

## Groups 

```agda
open import groups.abstract-abelian-groups
open import groups.abstract-abelian-subgroups
open import groups.abstract-group-actions
open import groups.abstract-group-torsors
open import groups.abstract-groups
open import groups.abstract-subgroups
open import groups.concrete-group-actions
open import groups.concrete-groups
open import groups.concrete-subgroups
open import groups.examples-higher-groups
open import groups.furstenberg-groups
open import groups.higher-groups
open import groups.sheargroups
```

## Order theory

```agda
open import order-theory.finite-posets
open import order-theory.finite-preorders
open import order-theory.finitely-graded-posets
open import order-theory.planar-binary-trees
open import order-theory.posets
open import order-theory.preorders
```

## Polytopes

```agda
open import polytopes.abstract-polytopes
```

## Rings

```agda
open import rings.eisenstein-integers
open import rings.gaussian-integers
open import rings.ideals
open import rings.localizations-rings
open import rings.rings-with-properties
open import rings.rings
```

## Synthetic homotopy theory

```agda
open import synthetic-homotopy-theory.23-pullbacks
open import synthetic-homotopy-theory.24-pushouts
open import synthetic-homotopy-theory.25-cubical-diagrams
open import synthetic-homotopy-theory.26-descent
open import synthetic-homotopy-theory.26-id-pushout
open import synthetic-homotopy-theory.27-sequences
open import synthetic-homotopy-theory.circle
open import synthetic-homotopy-theory.cyclic-types
open import synthetic-homotopy-theory.infinite-cyclic-types
open import synthetic-homotopy-theory.interval-type
open import synthetic-homotopy-theory.pointed-dependent-functions
open import synthetic-homotopy-theory.pointed-equivalences
open import synthetic-homotopy-theory.pointed-families-of-types
open import synthetic-homotopy-theory.pointed-homotopies
open import synthetic-homotopy-theory.pointed-maps
open import synthetic-homotopy-theory.pointed-types
open import synthetic-homotopy-theory.spaces
open import synthetic-homotopy-theory.universal-cover-circle
```

## Univalent combinatorics

```agda
open import univalent-combinatorics.2-element-types
open import univalent-combinatorics.binomial-types
open import univalent-combinatorics.cartesian-product-finite-types
open import univalent-combinatorics.coproduct-finite-types
open import univalent-combinatorics.counting-cartesian-product-types
open import univalent-combinatorics.counting-coproduct-types
open import univalent-combinatorics.counting-decidable-subtypes
open import univalent-combinatorics.counting-dependent-function-types
open import univalent-combinatorics.counting-dependent-pair-types
open import univalent-combinatorics.counting-fibers-of-maps
open import univalent-combinatorics.counting-function-types
open import univalent-combinatorics.counting-maybe
open import univalent-combinatorics.counting
open import univalent-combinatorics.decidable-dependent-function-types
open import univalent-combinatorics.decidable-dependent-pair-types
open import univalent-combinatorics.decidable-subtypes
open import univalent-combinatorics.dependent-product-finite-types
open import univalent-combinatorics.dependent-sum-finite-types
open import univalent-combinatorics.distributivity-of-set-truncation-over-finite-products
open import univalent-combinatorics.double-counting
open import univalent-combinatorics.embeddings
open import univalent-combinatorics.embeddings-standard-finite-types
open import univalent-combinatorics.equality-finite-types
open import univalent-combinatorics.equality-standard-finite-types
open import univalent-combinatorics.equivalences
open import univalent-combinatorics.equivalences-standard-finite-types
open import univalent-combinatorics.fibers-of-maps-between-finite-types
open import univalent-combinatorics.finite-choice
open import univalent-combinatorics.finite-connected-components
open import univalent-combinatorics.finite-function-types
open import univalent-combinatorics.finite-presentations
open import univalent-combinatorics.finite-types
open import univalent-combinatorics.finitely-presented-types
open import univalent-combinatorics.image-of-maps
open import univalent-combinatorics.inequality-types-with-counting
open import univalent-combinatorics.injective-maps
open import univalent-combinatorics.lists
open import univalent-combinatorics.maybe
open import univalent-combinatorics.pi-finite-types
open import univalent-combinatorics.pigeonhole-principle
open import univalent-combinatorics.ramsey-theory
open import univalent-combinatorics.retracts-of-finite-types
open import univalent-combinatorics.standard-finite-types
open import univalent-combinatorics.sums-of-natural-numbers
open import univalent-combinatorics.surjective-maps
```

## Univalent foundation

```agda
open import univalent-foundations.functoriality-loop-spaces
open import univalent-foundations.isolated-points
open import univalent-foundations.iterated-loop-spaces
open import univalent-foundations.loop-spaces
```

## Everything

See the list of all Agda modules [here](everything.html).

