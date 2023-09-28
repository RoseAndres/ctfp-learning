# Types and Functions
[Link to original post](https://bartoszmilewski.com/2014/11/24/types-and-functions/)  
[WBM link](https://web.archive.org/web/20230000000000*/https://bartoszmilewski.com/2014/11/24/types-and-functions/) (just in case)

## New Terms/Definitions
- _Set_: A collection of unique values. Can be finite (-5 <= x <= 5) or infinite (even numbers)
- _Type_: A _Set_ of specific values. `Bool` is a two-element set of `True` and `False`. `Char` is a set of all Unicode characters like `a` and `å`.
- _Bottom_: A special value that is added to every type, denoted by `_|_`. It corresponds to a non-terminating computation. Also evaluates to `undefined`
- _Predicates_: Functions to Bool
- _Partial Functions_: Functions that may return _Bottom_.
- _Total Functions_: Functions that return valid results for every possible argument.
- _Pure Functions_: Functions that always have no side effects and produce the same result given the same input.

## Notes
- Category theory is about composing arrows
- Arrows can only be composed if the target object of one arrow is the same as the source object of the next arrow.
- There is a _Category_ of _Sets_ called **Set**. In **Set**, objects are sets and morphisms are functions.
- Strict typing in combination with pure functions that can be equated to mathematical functions make it possible to write provable code.

## Challenges
