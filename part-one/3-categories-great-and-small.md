# 3. Categories Great and Small
[Link to original post](https://bartoszmilewski.com/2014/12/05/categories-great-and-small/)

## Definitions
- _Thin Category_: A category where there is at most one morphism going from any object a to any object b.
- _Hom-set_: A set of morphisms from object a to object b in a category C
- _Monoid_:
     - **As a set**: A set with a binary operation that is associative and has one special element that behaves like a unit with respect to that operation.
     - **As a category**: A single object category with a set of morphisms that follow appropriate rules of composition
- _Extensional Equality_: For any two input strings, two outputs of two functions (arrows) are the same.

## Notes

## Challenges
   1. Generate a free category from:
     1. A graph with one node and no edges
     1. A graph with one node and one (directed) edge (hint: this edge can be composed with itself)
     1. A graph with two nodes and a single arrow between them
     1. A graph with a single node and 26 arrows marked with the letters of the alphabet: a, b, c … z.
   1. What kind of order is this?
     1. A set of sets with the inclusion relation: A is included in B if every element of A is also an element of B.
     1. C++ types with the following subtyping relation: T1 is a subtype of T2 if a pointer to T1 can be passed to a function that expects a pointer to T2 without triggering a compilation error.
   1. Considering that Bool is a set of two values True and False, show that it forms two (set-theoretical) monoids with respect to, respectively, operator && (AND) and || (OR).
   1. Represent the Bool monoid with the AND operator as a category: List the morphisms and their rules of composition.
   1. Represent addition modulo 3 as a monoid category.
