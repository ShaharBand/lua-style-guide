<div align="center">
  <img style="display: block; -webkit-user-select: none; margin: auto; cursor: zoom-in; background-color: hsl(0, 0%, 90%);" src="/luaa.gif" width="250" height="250"/>
</div>

# Lua Style Guide

Inspired by Python PEP8, this style guide provides conventions and best practices for writing clean, readable, and maintainable Lua code. 
By adhering to these guidelines, you can ensure consistency across your codebase and enhance collaboration with other developers.

Feel free to fork this style guide and change to your own
liking, and file issues / pull requests if you have questions, comments, or if
you find any mistakes or typos.

[![License: MIT](https://img.shields.io/badge/License-MIT-green2.svg)](/blob/main/LICENSE)
[![last commit](https://img.shields.io/github/last-commit/ShaharBand/lua-style-guide.svg)](https://github.com/ShaharBand/lua-style-guide/commits/main) 
[![stars](https://img.shields.io/github/stars/ShaharBand/lua-style-guide.svg?style=badge)](https://github.com/ShaharBand/lua-style-guide/stargazers) 
<br> 

## Table of Contents
1. [Variables](#1-variables)
   1. [Naming Conventions](#11-naming-conventions)
   2. [Variable Scope](#12-variable-scope)
   3. [Constants](#13-constants)
2. [Statements](#2-statements)
   1. [If then else](#21-if-then-else)
   2. [Control Flow: Loops](#22-control-flow-loops)
3. [Functions](#3-functions)
   1. [Naming Conventions](#31-naming-conventions)
3. [Conclusions](#4-conclusions)
 
<br>

## 1. Variables


  ## 1.1 Naming Conventions
  
  - Use descriptive names that clearly indicate the variable's purpose.
  - Avoid single-letter names except for loop variables or trivial cases.
  - Follow consistent naming patterns to improve readability and maintainability.
  - Use `snake_case` for local variable names.
  - Use `UPPER_CASE_WITH_UNDERSCORES` for constant
    
  ```lua
  -- global
  MAX_USERS = 100
  PI = 3.14159
  
  -- local
  local user_name = "JohnDoe"
  local total_count = 0
  ```

Note: There is some confusion in Lua about variable naming conventions. In case of a framework or large projects, follow the already existing conventions according to the scope to avoid mixing new conventions.

  **[[⬆]](#table-of-contents)**
    
  ## 1.2 Variable Scope
  Besides global variables, Lua supports local variables. We create local variables with the local statement:
  ```lua
  j = 10         -- global variable
  local i = 1    -- local variable
  ```
    
  Unlike global variables, local variables have their scope limited to the block where they are declared. 
  A block is the body of a control structure, the body of a function, or a chunk (the file or string with the code where the variable is declared).
  
  It is good programming style to use local variables whenever possible. 
  Local variables help you avoid cluttering the global environment with unnecessary names. 
  Moreover, the access to local variables is faster than to global ones.
  
  ```lua
  variable = 1       -- bad
  local variable = 1 -- good
  ```
  
  Assign variables at the top of their scope where possible. This makes it easier to check for existing variables.
  
  ```lua
  -- bad example
  local bad = function()
    test()
    //..other stuff..
  
    local name = GetCurrentUsername()
  
    if name == 'test' then
      return false
    end
  
    return name
  end
  ```
  ```lua
  -- good example
  local function Good()
    local name = GetCurrentUsername()
  
    test()
    //..other stuff..
  
    if name == 'test' then
      return false
    end
  
    return name
  end
  ```
  **[[⬆]](#table-of-contents)**
  
  ## 1.3 Constants
  
  Constants are variables that should not change once they are set. 
  Using constants can make your code more readable and prevent accidental modifications. 
  In Lua, we typically follow the convention of naming them in `UPPER_CASE_WITH_UNDERSCORES`.
  
  Define constants at the top of the file or the top of their respective scope.
  Use constants to avoid magic numbers and hard-coded values within your code.
  Group related constants together for better organization and readability.
  
  ```lua
  -- Define constants at the top of the file
  MAX_HEALTH = 100
  INITIAL_SCORE = 0
  GAME_TITLE = "Super Lua Game"
  
  -- Function using constants
  local function InitializeGame()
      local player_health = MAX_HEALTH
      local player_score = INITIAL_SCORE
      print("Welcome to " .. GAME_TITLE)
  end
  
  InitializeGame()
  ```
  **[[⬆]](#table-of-contents)**
  
<br>

## 2. Statements
Lua provides a small and conventional set of control structures, with if for conditional and while, repeat, and for for iteration. 
All control structures have an explicit terminator: end terminates the if, for and while structures; and until terminates the repeat structure.

The condition expression of a control structure may result in any value. Lua treats as true all values different from false and nil.

  ## 2.1 If then else
  An if statement tests its condition and executes its then-part or its else-part accordingly. The else-part is optional.
  When you write nested ifs, you can use elseif. 
  It is similar to an else followed by an if, but it avoids the need for multiple ends:
  
  ```lua
  -- good
  if op == "+" then
    r = a + b
  elseif op == "-" then
    r = a - b
  elseif op == "*" then
    r = a*b
  elseif op == "/" then
    r = a/b
  else
    error("invalid operation")
  end
  ```
  **[[⬆]](#table-of-contents)**
    
  ## 2.2 Control Flow: Loops
  In Lua, there are two main types of loops: `while` and `repeat`. 
  Understanding the differences between them is essential for writing efficient and effective code.
  
  ### `while` Loop
  The `while` loop tests its condition before executing the loop's body. If the condition is `false`, the loop ends immediately. 
  If the condition is `true`, Lua executes the body and then repeats the test.
  
  ```lua
  while false do
    print("hi")
  end
  -- This will never print anything
  ```
  
  ### `repeat` Loop
  
  The `repeat` loop, also known as `repeat-until`, behaves differently. 
  It always executes its body at least once because the condition is tested after the body. 
  The loop continues until the condition evaluates to `true`.
  
  ```lua
  -- Print the first non-empty line
  repeat
    line = io.read()
  until line ~= ""
  print(line)
  ```
  
  ### Choosing the Right Loop
  Choosing between `while` and `repeat` can help avoid unnecessary code and ensure that your loops function as intended. 
  Use `while` when you need to test the condition before executing the loop body. 
  Use `repeat` when the loop body must execute at least once before the condition is tested.

  **[[⬆]](#table-of-contents)**
    
<br>

## 3. Functions


  ## 3.1 Naming Conventions

- Use `PascalCase` for function names.
- Use `snake_case` for parameters name.
    
  ```lua
  -- good
  function CalculatePlayerScore(player)  
    -- function logic
  end
  ```
  ```lua
  -- bad
  function calcScore(p)                  
    -- function logic
  end

  -- bad
  function calc_score(p)                  
    -- function logic
  end

  -- bad
  function CALC_SCORE(p)                  
    -- function logic
  end

  -- bad
  function CalculatePlayerScore(Player)                  
    -- function logic
  end
  ```

- Use descriptive names that clearly indicate the function's purpose.
  ```lua
  -- good
  function CalculatePlayerScore()   
      -- function logic
  end

  -- good
  function UpdateDatabaseRecord()
      -- function logic
  end
  ```
  ```lua
  --- bad
  function calcScore() 
      -- function logic
  end

  -- bad
  function updRec()        
      -- function logic
  end
  ```

  **[[⬆]](#table-of-contents)**

<br>
  
## 4. Conclusions
N/A.

  **[[⬆]](#table-of-contents)**

Mentions:
- https://www.lua.org/
- https://github.com/Olivine-Labs/lua-style-guide/
