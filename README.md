<div align="center">
  <img style="display: block; -webkit-user-select: none; margin: auto; cursor: zoom-in; background-color: hsl(0, 0%, 90%);" src="/luaa.gif" width="250" height="250"/>
</div>

# Lua Style Guide

This style guide for Lua is inspired by Python and aims to make your code more maintainable and clean.

[![License: MIT](https://img.shields.io/badge/License-MIT-green2.svg)](/blob/main/LICENSE)
[![last commit](https://img.shields.io/github/last-commit/ShaharBand/lua-style-guide.svg)](https://github.com/ShaharBand/lua-style-guide/commits/main) 
[![stars](https://img.shields.io/github/stars/ShaharBand/lua-style-guide.svg?style=badge)](https://github.com/ShaharBand/lua-style-guide/stargazers) 
<br> 

## Table of Contents
1. [Introduction](#1-introduction)
2. [Variables](#2-variables)
   1. [Naming Conventions](#21-naming-conventions)
   2. [Variable Scope](#22-variable-scope)
   3. [Constants](#23-constants)
3. [Statements](#3-statements)
   1. [If then else](#31-if-then-else)
   2. [Control Flow: Loops](#32-control-flow-loops)
3. [Conclusions](#4-conclusions)
 
<br>

## 1. Introduction
This style guide provides conventions and best practices for writing Lua code that is clean, readable, and maintainable. 
By following these guidelines, you can ensure consistency across your codebase and improve collaboration with other developers.
<br><br><br>

## 2. Variables


### 2.1 Naming Conventions
N/A.

### 2.2 Variable Scope
Besides global variables, Lua supports local variables. We create local variables with the local statement:

    j = 10         -- global variable
    local i = 1    -- local variable
    
Unlike global variables, local variables have their scope limited to the block where they are declared. 
A block is the body of a control structure, the body of a function, or a chunk (the file or string with the code where the variable is declared).

It is good programming style to use local variables whenever possible. 
Local variables help you avoid cluttering the global environment with unnecessary names. 
Moreover, the access to local variables is faster than to global ones.

    variable = 1       -- usually bad
    local variable = 1 -- good

Assign variables at the top of their scope where possible. This makes it easier to check for existing variables.
 
    -- bad example
    local bad = function()
      test()
      //..other stuff..

      local name = get_current_username()

      if name == 'test' then
        return false
      end

      return name
    end 
<br>

    -- good example
    local function good()
      local name = get_current_username()

      test()
      //..other stuff..

      if name == 'test' then
        return false
      end

      return name
    end

### 2.3 Constants
N/A.

<br>

## 3. Statements
Lua provides a small and conventional set of control structures, with if for conditional and while, repeat, and for for iteration. 
All control structures have an explicit terminator: end terminates the if, for and while structures; and until terminates the repeat structure.

The condition expression of a control structure may result in any value. Lua treats as true all values different from false and nil.

## 3.1 If then else
An if statement tests its condition and executes its then-part or its else-part accordingly. The else-part is optional.
When you write nested ifs, you can use elseif. 
It is similar to an else followed by an if, but it avoids the need for multiple ends:

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

## 3.2 Control Flow: Loops
In Lua, there are two main types of loops: `while` and `repeat`. 
Understanding the differences between them is essential for writing efficient and effective code.

### `while` Loop
The `while` loop tests its condition before executing the loop's body. If the condition is `false`, the loop ends immediately. 
If the condition is `true`, Lua executes the body and then repeats the test.

    while false do
      print("hi")
    end
    -- This will never print anything

### `repeat` Loop

The `repeat` loop, also known as `repeat-until`, behaves differently. 
It always executes its body at least once because the condition is tested after the body. 
The loop continues until the condition evaluates to `true`.

    -- Print the first non-empty line
    repeat
      line = io.read()
    until line ~= ""
    print(line)


### Choosing the Right Loop
Choosing between `while` and `repeat` can help avoid unnecessary code and ensure that your loops function as intended. 
Use `while` when you need to test the condition before executing the loop body. 
Use `repeat` when the loop body must execute at least once before the condition is tested.

<br>

## 4. Conclusions
N/A.

Mentions:
- https://www.lua.org/
- https://github.com/Olivine-Labs/lua-style-guide/
