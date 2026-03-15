# Pure Functions
Always produces the same result when given the same parameters
Never has side effects
Never alters state

> I/O actions are the only impure execution in Haskell
# IO Monad

> IO Monad let us write programs that do I/O in a strictly sequential, imperative manner
`(>>=) :: IO a -> (a-> IO b) -> IO b`  
  
`getChar >>= putChar`
`>> :: IO a-> IO b -> IO b`
## `do`
a syntax sugar for IO monads
```Haskell
getTwoChars :: IO (Char, Char)
getTwoChars = getChar >>= \c1 ->
	getChar >>= \c2 ->
	return (c1, c2)
-- do:
getTwoChars = do { c1 <- getChar;
	c2 <- getChar;
	return (c1, c2) }
```
# Writer Monad
```Haskell
newtype Writer w a = Writer { runWriter :: (a, w) }
instance (Monoid w) => Monad (Writer w) where
    return x = Writer (x, mempty)
    (Writer (x,v)) >>= f = let (Writer (y, v')) = f x in Writer (y, v `mappend` v')
```
# Reader Monad
# Monad Transformer
**monad transformers**: special types that allow us to roll two monads into a single one that shares the behavior of both.