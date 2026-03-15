Eslint(linter): point out why the format is wrong + code quality rules + prevent coding errors + offer options to fix
Prettier(formatter): formatting

> Use Prettier(formatters) for code formatting concerns, and linters for code-quality concerns  
> Combine them: Prettier is used as an  
> `extend` in Eslint.
Note: other linter: JSHint
# Files
.eslintignore
- .eslintrc
    
    env (Each environment brings with it a certain set of predefined global variables)
    
    globals
    
    - rules: which rules are enabled and at what error level
        
        extends: config files that add rules
        
    - plugins: which third-party plugins define additional rules, environments, configs, etc. for ESLint to use
        
        parser
        
        processor
        
- .prettierrc
    
    `Prettier intentionally doesn’t support any kind of global configuration.`
    
tsconfig.json