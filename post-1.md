# [Category: The Essence of Composition](https://bartoszmilewski.com/2014/11/04/category-the-essence-of-composition/)

## Definitions
* _Object_: An abstract entity (internals are unimportant to CT)
* _Morphism_: A connection/arrow/function that relates one _Object_ to another. It starts at one _Object_ (input) and ends at an _Object_ (output).
* _Composition_: In essence, refers to how the elements of a _Category_ relate to one another: A -> B, B -> C :: A -> C
* _Category_: A collection of _Objects_ with _Morphisms_ that relate them to one another. 
  _Categories_ have two rules that their composition must satisfy:
  1. The _Composition_ must be associative (if any _Morphisms_ can be composed, they don't need to be expressed with any parentheses)
  2. For every _Object_ **A** there must be a _Morphism_ that begins and ends on itself (an Identity function)
* _Identity Function_: A _Morphism_ that starts and ends on the same _Object_

## Notes
* In Haskell, a function can be defined with a name followed by a double-colon, then an input type followed by an output type
  ```haskell
    -- a function named 'f' with an input type of A and an output type of B 
    f :: A -> B
  ```
* Composition is indicated by a period between two _Morphisms_:
  ```haskell
    f :: A -> B
    g :: B -> C
    g . f
  ```
* Composition is read from right-to-left. So `g . f` can be thought of as _"g after f"_.
* Explanation of a Category's Composition needing to be _associative_:
  ```haskell
    f :: A -> B
    g :: B -> C
    h :: C -> D
    h . (g . f) == (h . g) . f == h . g . f
  ```
  Since the three functions can be composed, there is no need for parentheses.

## Challenges

