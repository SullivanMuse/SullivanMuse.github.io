---
title: "Orthogonal"
date: 2023-05-17T16:49:16-05:00
draft: false
---

There are a very small number of truly orthogonal ideas in programming language design. The point of this post is to document them.

## Interface

Interfaces go by many names, but all are interfaces:
- API
- ABI
- interface
- duck-typing
- 

## Hierarchical object store

Fundamentally, files are just objects. We really shouldn't need to implement a parser for each different file type, because there should be a one-to-one mapping between file types and plain-old-data file types.

interface of a tagged union
    type?
    getValue1
    getValue2
    getValue3

interface of an untagged union
    getValue1
    getValue2
    getValue3

interface of a bool
    isTrue
    isFalse

interface of a 