# Types
Bool  
Char, String (list of Char)  
Int (fixed precision), Integer (arbitrary precision)  
Float  
List `[]`, Tuple `()`
Function type:  
  
`length :: [a] -> int` (a is for polymorphism)  
  
`putStrLn :: String -> IO ()` (() means no return type): takes in a string, produce an _IO action_ without return
## Type Declaration (Type Synonyms)

> Motivation: readability and documentation
```Haskell
type MyString = [Char]
type Pair a = (a, a)
```
`type` can be nested, but not recursive:
```Haskell
-- nested
type Pos = (Int, Int)
type Trans = Pos ->Pos
-- recursive (WRONG!)
type Tree = (Tree, Int, Tree)
```
## Data Declaration

> Motivation: create new data type for complex data structures
```Haskell
data TypeName =  <constructor1> | <constructor2> | …
data Bool = False | True
data Shape = Circle Float | Rect Float Float
-- Circle :: Float -> Shape
-- Rect :: Float -> Float -> Shape
-- can be recursive
data Nat = Zero | Succ Nat
data Tree a = Leaf a | Node (Tree a) a (Tree a)
```

> also `newtype`: newtype can be replaced with data; data can only be replaced with newtype if the type has exactly one constructor with exactly one field inside it.
# List Operations
```Haskell
xs = [1..5] -- [1,2,3,4,5]
elem 3 xs -- True
length xs -- 5
0:xs -- [0,1,2,3,4,5]
-- slicing and concatenation
head xs -- 1
tail xs -- [2,3,4,5]
xs !! 2 -- second element
take 3 xs -- [1,2,3] take first 3 elements
drop 3 xs -- [4,5] remove first3 elements
xs ++ [6,7] -- [1,2,3,4,5,6,7]
concat [xs, [6,7]] -- [1,2,3,4,5,6,7]
-- advanced functions
reverse xs -- [5,4,3,2,1]
sum xs -- 15
product xs -- 120
zip [1,2,3] ['a', 'b', 'c'] -- [(1,'a'),(2,'b'),(3,'c')]
-- list comprehension
[x^2 | x <- xs] -- [1,4,9,16,25]
[(x,y) | x <- [1,2,3], y <-[4,5]] -- [(1,4),(1,5),(2,4),(2,5),(3,4),(3,5)]
	-- dependent generator
[(x,y) | x <- [1,2,3], y <-[x..3]] -- [(1,1),(1,2),(1,3),(2,2),(2,3),(3,3)]
concat xss = [x | xs <- xss, x <-xs]
	-- guard
factors n = [x | x <- [1..n], n `mod` x == 0]
```
# I/O Action
Have the type `IO` `_t_`
Produce an effect when _performed_, but not when _evaluated_
Any expression may produce an action as its value, but the action will not perform I/O until it is executed inside another I/O action (or it is `main`)
Performing (executing) an action of type `IO t` may perform I/O and will ultimately deliver a result of type `t`
```Haskell
name2reply :: String -> String
name2reply name =
    "Pleased to meet you, " ++ name ++ ".\n" ++
    "Your name contains " ++ charcount ++ " characters."
    where charcount = show (length name)
main :: IO ()
main = do
      putStrLn "Greetings once again.  What is your name?"
      inpStr <- getLine -- get value from actions
      let outStr = name2reply inpStr -- get value from simple code
      putStrLn outStr
```
**Local Bindings**:
```Haskell
let x = 3 in x + 5
x + 5 where x = 3
```
In do notation, `let` does not need an `in` part, the value will be applied to the rest of the do body by default.
# Conditional Expressions & Logic
```Haskell
-- 1
signum n = if n < 0 then -1 else
	if n == 0 then 0 else 1
-- 2 guarded equations
signum n | n < 0 = -1
	| n > 0 = 1
	| otherwise = 0
```
```Haskell
not && ||
```
# Functions

> `<function_name> <argument1> <argument2>...`  
> `function_name` must start with lower case letter

function application is the highest priority: `f a * b` means f(a) * b
**Anonymous Function**: `(\x y` `**->**` `x + y)`
**Function Composition**: `f . g = \x -> f (g x)`
## Infix Operators & Sections
``x `f` y`` is the same as `f x y`
`x + y` is the same as `(+) x y`
``isVowel = (`elem` "AEIOUaeiou")``
## Pattern Matching
```Haskell
f 1 = True
f _ = False -- use _ as a wild card
```
## Higher Order Functions
```Haskell
map f xs
filter f xs
-- folder functions
foldr f xs
sum = foldr (+) 0
or = foldr (||) 0
```