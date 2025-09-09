<div align="center">
  <img style="display: block; -webkit-user-select: none; margin: auto; cursor: zoom-in; background-color: hsl(0, 0%, 90%);" src="/luaa.gif" width="250" height="250"/>
</div>

# Lua Style Guide

Inspired by Python PEP8, this style guide provides conventions and best practices for writing clean, readable, and maintainable Lua code. 
By adhering to these guidelines, you can ensure consistency across your codebase and enhance collaboration with other developers.

Feel free to fork this style guide and change to your own
liking, and file issues / pull requests if you have questions, comments, or if
you find any mistakes or typos.

[![License: MIT](https://img.shields.io/badge/License-MIT-green2.svg)](LICENSE)
[![last commit](https://img.shields.io/github/last-commit/ShaharBand/lua-style-guide.svg)](https://github.com/ShaharBand/lua-style-guide/commits/main) 
[![stars](https://img.shields.io/github/stars/ShaharBand/lua-style-guide.svg?style=badge)](https://github.com/ShaharBand/lua-style-guide/stargazers) 
<br> 

## Table of Contents
1. [Variables](#1-variables)
   1. [Naming Conventions](#11-naming-conventions)
   2. [Variable Scope](#12-variable-scope)
   3. [Constants](#13-constants)
   4. [Strings](#14-strings)
   5. [Tables](#15-tables)
2. [Statements](#2-statements)
   1. [If then else](#21-if-then-else)
   2. [Control Flow: Loops](#22-control-flow-loops)
3. [Functions](#3-functions)
   1. [Naming Conventions](#31-naming-conventions)
4. [Formatting](#4-formatting)
   1. [Indentation and Whitespace](#41-indentation-and-whitespace)
   2. [Line Length and Breaks](#42-line-length-and-breaks)
   3. [Spacing](#43-spacing)
5. [Modules](#5-modules)
   1. [Module Pattern](#51-module-pattern)
   2. [Requiring and Locals](#52-requiring-and-locals)
6. [Error Handling](#6-error-handling)
   1. [pcall and xpcall](#61-pcall-and-xpcall)
   2. [Assertions](#62-assertions)
7. [Comments and Documentation](#7-comments-and-documentation)
   1. [Comments](#71-comments)
   2. [LDoc/EmmyLua](#72-ldocemmylua)
8. [Performance](#8-performance)
9. [Metatables and OOP](#9-metatables-and-oop)
10. [Coroutines](#10-coroutines)
11. [Tooling](#11-tooling)
12. [Testing](#12-testing)
 
<br>

## 1. Variables


  ## 1.1 Naming Conventions
  
  - Use descriptive names that clearly indicate the variable's purpose.
  - Single-letter variable names should be avoided except for very small scopes (less than ten lines) or for iterators.
  - `i` should be used only as a counter variable in for loops (either numeric for or `ipairs`).
  - Prefer more descriptive names than `k` and `v` when iterating with `pairs`, unless you are writing a function that operates on generic tables.
  - Use `_` for ignored variables.
  - Variable names with larger scope should be more descriptive than those with smaller scope. 
  - Follow consistent naming patterns to improve readability and maintainability.

  - Use `snake_case` for local variable names.
  - Use `UPPER_CASE_WITH_UNDERSCORES` for constants.

  ```lua
  -- global
  MAX_USERS = 100
  PI = 3.14159
  
  -- local
  local user_name = "JohnDoe"
  local total_count = 0
  ```


  > Note: There is some confusion in Lua about variable naming conventions. In case of a framework or large projects, follow the already existing conventions according to the scope to avoid mixing new conventions.

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
    -- other stuff

    local name = get_current_username()

    if name == 'test' then
      return false
    end

    return name
  end
  ```
  ```lua
  -- good example
  local function good()
    local name = get_current_username()

    test()
    -- other stuff

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
  local function initialize_game()
    local player_health = MAX_HEALTH
    local player_score = INITIAL_SCORE
    print("Welcome to " .. GAME_TITLE)
  end
  
  initialize_game()
  ```
  **[[⬆]](#table-of-contents)**

  ## 1.4 Strings
  
  - Prefer "double quotes" for general strings; use 'single quotes' when the string contains double quotes.
  - Use `string.format` for readability instead of many concatenations.
  - Avoid concatenation inside tight loops; collect parts and use `table.concat`.
  - Use long brackets `[[ ... ]]` for multi-line literals.
  - Escape only when needed (e.g., `\n`, `\t`, `\\`).
  
  ```lua
  local greeting = "Hello, \"Lua\""
  local msg = string.format("%s scored %d points", player_name, score)

  -- bad
  local msg = "Hello " .. name .. ", you have " .. count .. " messages."

  -- good
  local msg = string.format("Hello %s, you have %d messages.", name, count)

  -- bad (slow + messy)
  local result = ""
  for i = 1, 1000 do
    result = result .. i .. ","
  end

  -- good
  local parts = {}
  for i = 1, 1000 do
    parts[#parts+1] = string.format("%d,", i)
  end
  local result = table.concat(parts)

  -- bad
  local text = "This is line 1\nThis is line 2\nThis is line 3"

  -- good
  local text = [[
  This is line 1
  This is line 2
  This is line 3
  ]]

  ```


  ## 1.5 Tables
  
  The table type implements associative arrays. An associative array is an array that can be indexed not only with numbers, but also with strings or any other value of the language, except `nil`.

  - Initializing a table with populating its fields all at once, if possible.
  - Add a trailing comma to all fields, including the last one.
    
  > This makes the structure of your tables more evident at a glance, Trailing commas make it quicker to add new fields and produces shorter diffs.
  ```lua
  local player_data = {
    name = "Shahar",
    level = 12,
  }
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
    r = a * b
  elseif op == "/" then
    r = a / b
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

  - Use `snake_case` for function and parameters name.
  - Use descriptive names that clearly indicate the function's purpose.
  - Don’t use spaces between function name and opening round bracket. Split arguments with one whitespace character.
  ```lua
  -- good
  function calculate_player_score()   
    -- function logic
  end

  -- good
  function update_database_record(record)
    -- function logic
  end
  ```

  ```lua
  --- bad (function camelCase name)
  function calcScore() 
    -- function logic
  end

  -- bad (function undescriptive name)
  function updRec()        
    -- function logic
  end

  -- bad (function name PascalCase)
  function CalculateScore(p)                  
    -- function logic
  end

  -- bad (function name UPPER_SNAKE_CASE)
  function CALC_SCORE(p)                  
    -- function logic
  end

  -- bad (bad parameter name)
  function CalculatePlayerScore(Player)                  
    -- function logic
  end
  ```

  **[[⬆]](#table-of-contents)**

<br>
  
## 4. Formatting

  ### 4.1 Indentation and Whitespace
  - Use tabs for indentation not spaces manually.
  - Trim trailing whitespace; keep files UTF-8 without BOM.
  - One statement per line.

  ```lua
  -- bad (spaces)
  function greet()
    print("hello")
  end

  -- good (tabs)
  function greet()
	  print("hello")
  end
  ```

  ### 4.2 Line Length and Breaks
  - Soft limit lines to ~100–120 columns.
  - Break long expressions at operators, align continued lines.

  ```lua
  -- bad
  local msg = string.format("Hello %s, you have %d unread messages and %d notifications today.", name, unread, notifications)

  -- good
  local msg = string.format(
    "Hello %s, you have %d unread messages and %d notifications today.",
    name,
    unread,
    notifications
  )
  ```

  ### 4.3 Spacing
  - One space around binary operators (`=`, `+`, `..`, etc.).
  - No space after function names before `(`.
  - One blank line between top-level definitions.

  ```lua
  -- bad
  local sum = 1 + 2
  print (sum)
  function c () end

  -- good
  local sum = 1 + 2
  print(sum)

  function a() end

  function b() end
  ```

  **[[⬆]](#table-of-contents)**

<br>

## 5. Modules

  ### 5.1 Module Pattern
  - Prefer returning a table of functions/values from files.
  - Avoid polluting `_G` with globals.
  ```lua
  -- good
  local M = {}

  function M.add(a, b)
    return a + b
  end

  return M

  --  bad (puts function in _G)
  function add(a, b)
    return a + b
  end

  ```

  ### 5.2 Requiring and Locals
  - Assign `require` results to locals for speed and clarity.
  ```lua
  local utils = require("utils")
  local trim = utils.trim
  ```

  - Prefer explicit imports over pulling from `_G`.
  ```lua
  -- bad
  result = some_global_lib.process(data)

  -- good
  local lib = require("some_global_lib")
  local result = lib.process(data)
  ```

  **[[⬆]](#table-of-contents)**

<br>

## 6. Error Handling

  ### 6.1 pcall and xpcall
  - Use `pcall`/`xpcall` for recoverable failures; avoid bare errors in libraries.
  ```lua
  local ok, result = pcall(do_work)
  if not ok then
    log_error(result)
  end
  ```

  ```lua
  -- with traceback
  local ok, err = xpcall(do_work, debug.traceback)
  if not ok then
    log_error(err)
  end
  ```

  ### 6.2 Assertions
  - Use `assert` for programmer errors and preconditions.
  ```lua
  local file = assert(io.open(path, "r"))
  ```

  **[[⬆]](#table-of-contents)**

<br>

## 7. Comments and Documentation

  ### 7.1 Comments
  - Use `--` for single-line comments; `--[[ ... ]]` for multi-line.
  - Explain "why" not "what"; keep comments up to date.
  - Comments should be avoided if the code is clear

  ### 7.2 LDoc/EmmyLua
  - Document public APIs with LDoc/EmmyLua-style annotations for better tooling.
  ```lua
  --- Calculates the area of a circle
  -- @param r number radius
  -- @return number area
  local function circle_area(r)
    return math.pi * r * r
  end
  ```

  **[[⬆]](#table-of-contents)**

<br>

## 8. Performance
  - Prefer locals over globals (faster lookups).
  - Hoist invariants out of loops; cache `#t` when needed.
  - Use `table.concat` for building strings.
  - Pre-size arrays when possible: `local t = table.create and table.create(n) or {}`.

  ```lua
  -- bad (global lookup each time)
  for i = 1, n do
    total = total + math.abs(values[i])
  end

  -- good (cache locals)
  local abs = math.abs
  for i = 1, n do
    total = total + abs(values[i])
  end
  ```

  **[[⬆]](#table-of-contents)**

<br>

## 9. Metatables and OOP
  - Keep metatable usage minimal and explicit.
  - Use `__index` for method lookup; avoid magic unless well-documented.
  ```lua
  local Point = {}
  Point.__index = Point
  function Point.new(x, y)
    return setmetatable({ x = x, y = y }, Point)
  end
  function Point:len()
    return math.sqrt(self.x * self.x + self.y * self.y)
  end
  ```

  ```lua
  -- bad (implicit globals, hidden metatable behavior)
  P = {}
  setmetatable(P, { __index = function() return 0 end })
  ```

  **[[⬆]](#table-of-contents)**

<br>

## 10. Coroutines
  - Use coroutines to model producers/consumers and cooperative tasks.
  - Keep coroutine lifecycles clear; avoid deep nesting.
  ```lua
  local function generator(n)
    return coroutine.wrap(function()
      for i = 1, n do coroutine.yield(i) end
    end)
  end
  ```

  ```lua
  -- consumer
  local next_num = generator(3)
  print(next_num()) -- 1
  print(next_num()) -- 2
  print(next_num()) -- 3
  ```

  **[[⬆]](#table-of-contents)**

<br>

## 11. Tooling
  - Formatter: stylua; Linter: luacheck; Docs: LDoc.
  - Configure CI to run formatting and linting.
  ```bash
  stylua .
  luacheck .
  ```

  ```lua
  -- .luacheckrc (example)
  -- globals = { "vim" }
  -- ignore  = { "631" } -- line too long (if policy allows)
  ```

  **[[⬆]](#table-of-contents)**

<br>

## 12. Testing
  - Use busted or luaunit; structure tests mirroring module layout.
  - Keep tests deterministic; avoid global state.
  ```lua
  describe("math utils", function()
    it("adds", function()
      assert.are.equal(3, add(1, 2))
    end)
  end)
  ```

  **[[⬆]](#table-of-contents)**

<br>

Mentions:
- https://www.lua.org/
- https://github.com/Olivine-Labs/lua-style-guide/
