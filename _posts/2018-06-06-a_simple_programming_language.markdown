---
layout: post
title:      "A Simple Programming Language"
date:       2018-06-06 21:37:02 +0000
permalink:  a_simple_programming_language
---

In learning programming language theory, one of the first things you will want to do is to implement the simplest possible language you can think of. This language will only have a handful of terms.  They will be those for true and false, an if-then-else statement, a value-of-0 term, a successor and predecessor function that take a term (think i += 1 and i -= 1), 
and a function *iszero* that takes a term t and returns true if it's zero. The list of all terms is this:
```
t ::=
  true                                                              *constant true*
  false                                                            *constant false*
  if t then t else t                                                *conditional*
  0                                                                *constant zero*
  successor t                                                       *successor*
  predecessor t                                                   *predecessor*
```
			
In this list any term in the list can be substituted into a term that takes a term t. The format which I used to list the terms is called BNF (Backus normal forms). Italicized words are comments. **t**, the variable in the above list, is called a *metavariable* because it is not a variable in the actual object language, only it is only a variable in the language description. If you think about the language as a set of terms, you can inductively define the language as the set of terms is the smallest set T such that 
1. {true, false, 0} ⊆ T ; 
2. if t1 ∈ T , then {succ t 1, pred t1, iszero t1} ⊆ T ; 
3.  if t1 ∈ T , t2 ∈ T , and t3 ∈ T , then if t1 then t2 else t3 ∈ T .

There are many semantic styles but I will only give the operational semantics, which defines the behavior of a programming language by defining a simple abstract machine for it, with abstract meaning we are not defining low level detail, only the high level behaviors.
![The Arithmetic Language](https://imgur.com/a/egLjEkZ)

The right hand column is called *evaluation relations* on terms, written t->t', meaning t evaluates to t' in one step. For E-IfTrue, the machine can throw away t2 and be left with t1. For E-If, if the guard evaluates t1' then the whole conditional *if t1 then t2 else t3* evaluates to *if t1' then t2 else t3*. True and false do not evaluate anything. 

Programs such as *if false then 0 else (succ 0)* would return 1. *iszero (pred (succ (pred (succ 0)))* would return *true*. Also nonsensical expressions such as *if (succ 0) then 0 else false* would be legal, but we don't need to deal with those nonsensical cases yet. 
      
*Note:* Though I use Haskell in the following code samples, most functional languages will do the trick, and you could probably also do it in an imperative or object oriented language. First, I define the syntax of the language as a type term in Haskell: 

```
{-# LANGUAGE DeriveAnyClass #-} 
{-# LANGUAGE TemplateHaskell #-} 

module Syntax where
import Data.DeriveTH

data Term = TmTrue
          | TmFalse
          | TmIf Term Term Term
          | TmZero
          | TmSucc Term
          | TmPred Term
          | TmIsZero Term
          deriving (Show, Read, Eq, Ord)

$( derive makeArbitrary ''Term)
```

Then for evaluation, we make a isNumericVal function and an isVal function to check whether a term is a numeric value or a value respectively. The eval1 function then defines how evaluations work in our simple language and then the function eval utilizes that 

```
module Evaluate where

import Syntax
import Data.Maybe (fromMaybe)

isNumericValue :: Term -> Bool
isNumericValue TmZero = True
isNumericValue (TmSucc t) = isNumericValue t
isNumericValue _ = False

isval :: Term -> Bool
isval TmTrue = True
isval TmFalse = True
isval t = isNumericValue t

eval1 :: Term -> Maybe Term
-- We return t2 when the guard is TmTrue
eval1 (TmIf TmTrue t2 t3) = Just t2
-- We return t2 when the guard is TmFalse
eval1 (TmIf TmFalse t2 t3) = Just t3
-- Depending on the value of t1, we either return t2 or t3
eval1 (TmIf t1 t2 t3) =
  case eval1 t1 of
    Nothing -> Nothing
    Just t' -> Just (TmIf t' t2 t3)
-- Depending on what t1 is, we return something (not nothing)
-- if we can take the step t -> t' and then we return the value
-- of successor of t'
eval1 (TmSucc t1) = 
  case eval1 t1 of
    Nothing -> Nothing 
    Just t' ->  Just (TmSucc t')
-- The Predecessor of Zero in our language doesn't go into 
-- negative numbers but only returns Zero
eval1 (TmPred TmZero) = Just TmZero
-- The predecessor of the successor of a numeric value should
-- be itself
eval1 (TmPred (TmSucc nv1)) | isNumericValue nv1 = Just nv1
-- Depending on what t1 is, we return something (not nothing)
-- if we can take the step t -> t' and then we return the value
-- of predecessor of t'
eval1 (TmPred t1) = 
  case eval1 t1 of
    Nothing -> Nothing
    Just t' -> Just (TmPred t')
-- TmIsZero of zero should return true
eval1 (TmIsZero TmZero) = Just TmTrue
-- The successor of a numeric value should never be zero
eval1 (TmIsZero (TmSucc nv1)) | isNumericValue nv1 = Just TmFalse
-- Depending on what t1 is, we return something (not nothing)
-- if we can take the step t -> t' and then we return the boolean
-- of isZero of t'
eval1 (TmIsZero t1) = 
  case eval1 t1 of
    Nothing -> Nothing
    Just t' -> Just (TmIsZero t')
-- if the function gets any other term, return nothing
eval1 _ = Nothing

-- Use eval1 to return the term that results from the call to eval1
eval :: Term -> Term
eval t = 
  case eval1 t of
    Nothing -> t
    Just t' -> eval t'
```

Although our implementation does not cover the lexing and parsing, which would be beyond the scope of this post, you can use this main function to evaluate expressions: 

```
{-# LANGUAGE ScopedTypeVariables #-} 
module Main where

import Syntax
import Evaluate

main :: IO ()
main = do
  t :: Term <- readLn
  print (eval t)
```

Now running the main method and typing in the terms (verbatim as we don't have any form of parsing) from the definition of this tiny language, we can see the results as expected!
