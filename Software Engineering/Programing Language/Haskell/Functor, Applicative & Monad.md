# Functor
`fmap :: f => (a -> b) -> f a -> f b`  
infix version of fmap:  
`<$>`  
Apply a unary function to a wrapped variable:  
`(+5) <$> Just 3`
---
A data type implements the type class **Functor** if it defines how `fmap` applies to it  
(so fmap itself is defined by function pattern matching)  
---
Lists are functors: `fmap = map` functions are functors: `fmap f g = f . g`
# Applicative
`(<*>) :: f => f (a -> b) -> f a -> f b`  
Apply a wrapped function to a wrapped variable:  
`Just (+5) <*> Just 3` .
`pure :: f => a -> f a`  
Wrap a variable  
---
A data type implements the type class **Applicative** if it defines how `(<*>)` and `pure` applies to it
Applicative is a subset of functor: `f <$> x = pure f <*> x`
Applicative is more powerful than functor because it allows a function with multiple wrapped variable as arguments by currying: `pure (*) <*> Just 5 <*> Just 3`
---
List is an applicative: `pure x = [x]` `fs <*> xs = [f x | f <- fs, x <- xs]`  
Function is an applicative:  
`pure x = \_->x` `(f <*> g) x = f x (g x)`
# Monad
Bind: `(>>=) :: m => m a -> (a-> m b) -> m b`  
Apply a function that returns a wrapped variable to a wrapped variable. Apply  
`(a-> f b)` to `f a >>=` is **right associative** in haskell
`return :: m => a -> m a`  
Wrap a variable (return is pure, the reason for different names is purely historical)  
---
A data type implements the type class **Monad** if it defines how `(>>=)` and `return` applies to it
Monad is a subset of applicative: `mf <*> mx = mf >>= \f -> mx >>= \x -> return (f x)`
Monad is more powerful than applicative because it has the ability to use all the output from previous computations to decide what computations to run next. Applicative only allows using output from the very last computation.
```Haskell
ifM :: Maybe Bool -> Maybe a -> Maybe a -> Maybe a
ifM mbool th el = mbool >>= \bool -> if bool then th else el
-- impossible for functors: ifA (Just True) (Just 1) (Nothing) is Just (Just 1)
ifF c x y = (\c'-> if c' then x else y) <$> c
-- impossible for applicatives: ifA (Just True) (Just 1) (Nothing) is Nothing
ifA c x y = (\c' x' y' -> if c' then x' else y') <$> c <*> x <*> y
```
---
List is a monad: `return x = [x]` `xs >>= f = [y | x <- xs, y <- f x]`  
Function is a monad:  
`return x = \_->x` `f >>= g = \x -> g (f x) x`
```Haskell
-- Maybe is a monad
data Maybe a = Just a | Nothing
instance Monad(Either e) where
	return = Just
	Nothing >>= f = Nothing
	Just x >>= f = f x
-- Either is a monad
data Either a b = Left a | Right b
instance Monad(Either e) where
	return = Right
	Left a >>= _ = Left a
	Right b >>= f = f b
```
## Monad Laws

> a monad a monoid in the category of endofunctors
Left identity: `return a >>= f == f a`
Right identity: `m >>= return == m`
Associative: `(m >>= f) >>= g == m >>= (\x -> f x >>= g)`
### Join
`join :: m => m m a -> m a`  
  
`x >>= f = join (fmap f x)`
## More Operators
### Combinator
`(>>) :: m => m a -> m b -> mb`  
  
`**>>**` can be thought of as a special case of `**>>=**` where the result of the first action is ignored  
Use case: sequence two actions while disregarding any result produced by the first action  
```Haskell
echoDup :: IO ()
echoDup = getChar >>= (\c ->
	putChar c >>= (\() ->
	putChar c)) -- it is nested like this to allow later (a-> IO b) to access c
-- can be rewritten as:
echoDup = getChar >>= \c ->
	putChar c >>
	putChar c
```
### Left Bind
`(=<<) :: m => (a-> m b) -> m a -> m b`