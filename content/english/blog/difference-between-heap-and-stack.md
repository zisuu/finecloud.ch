---
title: "Difference Between Heap And Stack"
meta_title: "Difference Between Heap And Stack"
description: "In this Post I try to explain the difference between the heap and the stack. It's just a very high level amateur explanation, there is a lot more to read and learn about this Topics. This Drawing helps me a lot to understand the basic difference between the heap and the stack and as well how they interact together."
date: 2023-03-29T20:14:00
categories: ["Software Development"]
author: "David"
tags: ["heap", "stack", "memory"]
draft: false
---

In this Post I try to explain the difference between the heap and the stack. It's just a very high level amateur explanation, there is a lot more to read and learn about this Topics.

## Overview

This Drawing helps me a lot to understand the basic difference between the heap and the stack and as well how they interact together.

![Heap and Stack](https://www.finecloud.ch/media/posts/75/HeapAndStack-2.png)

The Heap is unstructured, this means that the Objects are laying somewhere around in the memory.

## Key Difference Between Stack and Heap Memory

- Stack is a linear data structure whereas Heap is a hierarchical data structure.
- Stack memory will never become fragmented whereas Heap memory can become fragmented as blocks of memory are first allocated and then freed.
- Stack accesses local variables only while Heap allows you to access variables globally.
- Stack variables can’t be resized whereas Heap variables can be resized.
- Stack memory is allocated in a contiguous block whereas Heap memory is allocated in any random order.
- Stack doesn’t require to de-allocate variables whereas in Heap de-allocation is needed.
- Stack allocation and deallocation are done by compiler instructions whereas Heap allocation and deallocation is done by the programmer.

## What is a Stack?

A stack is a special area of computer’s memory which stores temporary variables created by a function. In stack, variables are declared, stored and initialized during runtime.

It is a temporary storage memory. When the computing task is complete, the memory of the variable will be automatically erased. The stack section mostly contains methods, local variable, and reference variables.

## What is Heap?

The heap is a memory used by programming languages to store global variables. By default, all global variable are stored in heap memory space. It supports Dynamic memory allocation.

The heap is not managed automatically for you and is not as tightly managed by the CPU. It is more like a free-floating region of memory.

