# Category: The Essence of Composition
Link to original post: https://bartoszmilewski.com/2014/11/04/category-the-essence-of-composition/
WBM Link (just in case): https://web.archive.org/web/20230000000000*/https://bartoszmilewski.com/2014/11/04/category-the-essence-of-composition/


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
1. Implement, as best as you can, the identity function in your favorite language (or the second favorite, if your favorite language happens to be Haskell).
   ```ruby
     def identity(x)
       return x
     end
   ```
   
1. Implement the composition function in your favorite language. It takes two functions as arguments and returns a function that is their composition.
    ```ruby
      def compose(obj, fn_b, fn_a)
        fn_b.call(fn_a.call(obj))
      end
  
      def first_upcase(x)
        x.tap { |_x| _x[0] = _x[0].upcase }
      end
  
      def last_three(x)
        x[-3..-1]
      end 
  
      compose("hello", method(:first_upcase), method(:last_three)) # => "Llo"
      compose("hello", method(:last_three), method(:first_upcase)) # => "llo"
    ```
1. Write a program that tries to test that your composition function respects identity.
    ```ruby
      def compose(obj, fn_b, fn_a)
        fn_b.call(fn_a.call(obj))
      end
  
      def last_three(x)
        x[-3..-1]
      end
  
      def identity(x)
        x
      end
  
      id_comp_fn = compose("hello", method(:identity), method(:last_three)) # => "llo"
      fn_comp_id = compose("hello", method(:last_three), method(:identity)) # => "llo"
  
      id_comp_fn == fn_comp_id # => true
    ```
1. Is the world-wide web a category in any sense? Are links morphisms?  
  I think this falls apart for the same reason as the Facebook question...
1. Is Facebook a category, with people as objects and friendships as morphisms?  
    No it is not. The composition requirements fall apart pretty quickly.  
    1. Amanda being friends with Sarah, and Sarah being friends with Mark, does not guarantee that Amanda is friends with Mark (she **_hates_** Mark).
    1. The identity Morphism could be assumed I guess, but there's no actual (afaik) self relationship that indicates one is a friend unto themselves.
1. When is a directed graph a category?  
  Assuming that an edge/transition represents a Morphism, a directed graph could be considered a Category under the following conditions:  
    1. For each graph node _N_, there is an edge from that node to itself (begins and ends on _N_)
    1. For every path _P_ composed of edges that begins on a node _N_ and ends on a different node _M_, there is an edge from _N_ to each of the other Nodes along the path to- and including - _M_.
   

