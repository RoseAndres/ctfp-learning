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
- _Void_: The _Type_ that contains no values; corresponds to the Empty set (_{}_).
- _a_: A _Type_ variable that can stand for any _Type_.
- _absurd_: A function that takes a _Void_ argument as input. This is impossible, since a value of type _Void_ would need to be given, and there is no such value, since _Void_ contains none.
- _unit_ or _()_: The _type_ that corresponds to a singleton set. It's a _Type_ that has only one possible value. It simply "is". Thie value is essentially a dummy value. We don't care what it is and don't have to look at it, and there will only ever be one.
- _Parametrically Polymorphic_: Functions that can be implemented with the same formula for any type. For example: 
   ```haskell
      unit :: a -> ()
      unit _ = ()
   ```

## Notes
- Category theory is about composing arrows.
- Arrows can only be composed if the target object of one arrow is the same as the source object of the next arrow..
- There is a _Category_ of _Sets_ called **Set**. In **Set**, objects are sets and morphisms are functions.
- Strict typing in combination with pure functions that can be equated to mathematical functions make it possible to write provable code.
- Every function of _unit_ is equivalent to picking a single element from the target _Type_.
- Functions from _unit_ to any _Type_ A are in one-to-one correspondence with the elements of that set A.
- A function from a _Set_ A to a singleton set maps every element of A to the single element of that singleton set. For every A there is exactly one such function.
- Pure functions from Bool just pick two values from the target type, one corresponding to `True` and another to `False`.
- A function from _unit_ (the `void` type in Haskell) can always be called.

## Challenges
1. Define a higher-order function (or a function object) `memoize` in your favorite language. This function takes a pure function `f` as an argument and returns a function that behaves almost exactly like `f`, except that it only calls the original function once for every argument, stores the result internally, and subsequently returns this stored result every time it’s called with the same argument. You can tell the memoized function from the original by watching its performance. For instance, try to memoize a function that takes a long time to evaluate. You’ll have to wait for the result the first time you call it, but on subsequent calls, with the same argument, you should get the result immediately.
   ```ruby
      # can't take full credit for this, Copilot suggested it and I hadn't seen lambdas used as closures before, so I accepted it and dug into it to understand.
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
   ```ruby
      # using memoize function from previous question
      mem_rand = memoize(method(:rand))

      rand(1..10) => 2
      rand(1..10) => 8
   
      mem_rand.call(1..10) => 4 (will be a random number)
      ...
      mem_rand.call(1..10) => 4
      ...
      mem_rand.call(1..10) => 4
   ```
   No, `mem_rand` no longer produces a random number after the first time it is called, since it has cached the answer for that given set of arguments.
1. Most random number generators can be initialized with a seed. Implement a function that takes a seed, calls the random number generator with that seed, and returns the result. Memoize that function. Does it work?
   ```ruby
      def seed_rand(seed, range)
         srand(seed)
         rand(range)
      end

      # using memoize function from previous question
      mem_seed_rand = memoize(method(:seed_rand))

      seed_rand(12345, 1..10) => 3
      seed_rand(12345, 1..10) => 3
      mem_seed_rand.call(12346, 1..10) => 1 (random number w/ first seed)
      mem_seed_rand.call(1234, 1..10) => 4 (random number w/ second seed)
      mem_seed_rand.call(12346, 1..10) => 1
      mem_seed_rand.call(1234, 1..10) => 4
   ```
   Yes, it works now that we are memoizing the seed.
1. Which of these C++ functions are pure? Try to memoize them and observe what happens when you call them multiple times: memoized and not.
   
    1. The factorial function from the example in the text.
       Pure: 5! will always be 5! and not cause any side-effects
    1. ```C++
         std::getchar()
       ```
       Dirty: the output depends on the state of the computer
    1. ```C++
         bool f() { 
           std::cout << "Hello!" << std::endl;
           return true; 
         }
       ```
       Dirty: the output is always the same, but there is a side-effect of printing to the console.
    1. ```C++
         int f(int x)
         {
           static int y = 0;
           y += x;
           return y;
         }
       ```
       Dirty: the static variable `y` will maintain its value across calls to `f`. Calling `f(3)` 3 times would result in 3 different return values of `3`, `6`, and `9`.
1. How many different functions are there from Bool to Bool? Can you implement them all?
   ```haskell
      not :: Bool -> Bool
      not x = !x

      id :: Bool -> Bool
      id x = x

      true :: Bool -> Bool
      true _ = True

      false :: Bool -> Bool
      false _ = False   
   ```
1. Draw a picture of a category whose only objects are the types Void, () (unit), and Bool; with arrows corresponding to all possible functions between these types. Label the arrows with the names of the functions.
   
   My drawing is missing the following functions:
   ```haskell
      bool2void :: Bool -> Void
      u2void :: () -> Void
   ```
   Compare to [this answer](https://docs.google.com/document/u/1/d/1NcZ07pwYLbQif9hYqY8d82dh2FMf-6b-HXDuccWKfx8/pub)([WBM](https://web.archive.org/web/20230000000000*/https://docs.google.com/document/u/1/d/1NcZ07pwYLbQif9hYqY8d82dh2FMf-6b-HXDuccWKfx8/pub)) submitted by another reader.
   
   Also, the absurd functions, while they are able to be defined, are unable to be called, so they can essentially be ignored.
   
   <img src="https://github.com/RoseAndres/ctfp-learning/assets/32444239/5e9638c0-8723-4f03-b8cd-5271fdb18c17" width="50%">  
   
   
