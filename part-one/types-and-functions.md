# Types and Functions
[Link to original post](https://bartoszmilewski.com/2014/11/24/types-and-functions/)  
[WBM link](https://web.archive.org/web/20230000000000*/https://bartoszmilewski.com/2014/11/24/types-and-functions/) (just in case)

## New Terms/Definitions
- _Set_: A collection of unique values. Can be finite (-5 <= x <= 5) or infinite (even numbers).
- _Type_: A _Set_ of specific values. `Bool` is a two-element set of `True` and `False`. `Char` is a set of all Unicode characters like `a` and `å`.
- _Bottom_: A special value that is added to every type, denoted by `_|_`. It corresponds to a non-terminating computation. Also evaluates to `undefined`.
- _Predicates_: Functions that map to Bool.
- _Partial Functions_: Functions that may return _Bottom_.
- _Total Functions_: Functions that return valid results for every possible argument.
- _Pure Functions_: Functions that always have no side effects and produce the same result given the same input.

## Notes
- Category theory is about composing arrows.
- Arrows can only be composed if the target object of one arrow is the same as the source object of the next arrow..
- There is a _Category_ of _Sets_ called **Set**. In **Set**, objects are sets and morphisms are functions.
- Strict typing in combination with pure functions that can be equated to mathematical functions make it possible to write provable code.

## Challenges
1. Define a higher-order function (or a function object) `memoize` in your favorite language. This function takes a pure function `f` as an argument and returns a function that behaves almost exactly like `f`, except that it only calls the original function once for every argument, stores the result internally, and subsequently returns this stored result every time it’s called with the same argument. You can tell the memoized function from the original by watching its performance. For instance, try to memoize a function that takes a long time to evaluate. You’ll have to wait for the result the first time you call it, but on subsequent calls, with the same argument, you should get the result immediately.
   ```ruby
      # can't take full credit for this, Copilot suggested it and I hadn't seen lambdas used as a closure before, so I accepted it and dug into it to understand.
      def memoize(fn)
         cache = Hash.new { |h, k| h[k] = {} }
         lambda do |*args|
            cache[args][fn] ||= fn.call(*args)
         end
      end

      def sum(n)
        (1..n).to_a.sum
      end

      mem_sum = memoize(method(:sum))

      sum(1_000_000_000) # => takes > 10 seconds
      mem_sum.call(1_000_000_000) # => same as above  
      mem_sum.call(1_000_000_000) # => instant
   ```
1. Try to memoize a function from your standard library that you normally use to produce random numbers. Does it work?
1. Most random number generators can be initialized with a seed. Implement a function that takes a seed, calls the random number generator with that seed, and returns the result. Memoize that function. Does it work?
1. Which of these C++ functions are pure? Try to memoize them and observe what happens when you call them multiple times: memoized and not.
   
    1. The factorial function from the example in the text.
    1. ```C++
         std::getchar()
       ```
    1. ```C++
         bool f() { 
           std::cout << "Hello!" << std::endl;
           return true; 
         }
       ```
    1. ```C++
         int f(int x)
         {
           static int y = 0;
           y += x;
           return y;
         }
       ```
1. How many different functions are there from Bool to Bool? Can you implement them all?
1. Draw a picture of a category whose only objects are the types Void, () (unit), and Bool; with arrows corresponding to all possible functions between these types. Label the arrows with the names of the functions.
