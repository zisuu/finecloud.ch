---
title: "Awk And Sed"
meta_title: "Awk And Sed"
description: "awk and sed are text manipulation programs. You can use them to replace strings, change order, and perform calculations. Learn the basics of awk and sed in this guide."
date: 2024-04-05T13:52:00
categories: ["Linux", "Shell","Scripting"]
author: "David"
tags: ["awk", "sed", "bash", "linux", "shell"]
draft: false
---

awk and sed are text manipulation programs. You can use them, for example, to replace strings:

```bash
echo image.jpg | sed 's/\.jpg/.png/'
image.png
```

or change the order of strings:

```bash
echo "hello world" | awk '{print $2, $1}'
world hello
```

awk and sed are harder to learn than some other basic Linux CLI tools because they contain their own small programming language.

## Awk Basics

awk transforms lines of text (stdin) into any other text, using a set of instructions (called the awk program):

```bash
awk program input-files
```

An awk program can also contain one or more `actions`, for example, calculating values or printing text. These `actions` run only when an input matches a `pattern`:

```bash
pattern {action}
```

Typical patterns include:

- `BEGIN` runs once before awk processes any input
- `END` runs once after awk has processed all the input
- A regex surrounded by forward slashes, example: `/^[A-Z]/`
- Other awk-specific expressions, example: `FNR>5` tells awk to skip the first five lines of input

A `pattern` with no `action` runs the default action `{print}`.

awk can also perform calculations, such as:

```bash
seq 1 100 | awk '{s+=$1} END {print s}'
5050
```

(This sums numbers from 1 to 100.)

## Sed Basics

sed, like awk, can be used to transform text from stdin to any other text using instructions called `sed scripts`:

```bash
sed script input-files
```

The most common use case for sed is text replacement, as in this example:

```bash
echo "Windows eats Linux for breakfast" | sed s/Windows/Linux/g | sed s/Linux/Windows/2
```

This article was last updated on **April 5, 2024**.

