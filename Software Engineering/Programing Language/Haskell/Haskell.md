> compiler: ghc  
> interactive environment: ghci  

**GHCI:**
`ghci filename.hs` to load from any script
`:?` show all script
`:i` show info about
`:quit` quit ghci
`:type` _`expr`_ check type
`:reload` to update the script when there is a change
`:{` to start multiple-line code, `:}` to end it
**GHC:**
The file to compile should define `main :: IO()` , for example `main= print("hello world")`
compile: `ghc -o MyExecutable MyProgram.hs`
run code directly: `runghc MyProgram.hs`
[[Haskell Grammar]]
[[Functor, Applicative & Monad]]
[[Monad Family]]
  
More: traversable/foldable, lens