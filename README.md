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
    
<br>

## 4. Conclusions
N/A.
