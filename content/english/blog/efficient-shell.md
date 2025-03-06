---
title: "Working Efficiently Within Different Directories in a Linux Shell"
meta_title: "Efficient Directory Navigation in Linux Shell Using pushd and popd"
description: "Learn how to navigate directories efficiently in a Linux shell using the directory stack with pushd, popd, and dirs commands."
date: 2023-12-10T12:05:00Z
categories: ["Linux", "Shell"]
author: "David"
tags: ["bash", "linux", "shell"]
draft: false
---

## Introduction

Suppose you need to perform work in multiple directories:

- `/var/www/html`
- `/etc/apache2`
- `/etc/ssl/certs`
- `~/Work/Projects/Web/src`

You might use the `cd` command, but repeatedly typing it is inefficient:

```bash
$ cd ~/Work/Projects/Web/src
$ cd /var/www/html
$ cd /etc/apache2
$ cd ~/Work/Projects/Web/src
$ cd /etc/ssl/certs
```

A more efficient approach is using the **directory stack**.

## Directory Stack

The directory stack allows **pushing** and **popping** directories.

- **Pushing** a directory adds it to the top of the stack.
- **Popping** removes the topmost directory.
- Initially, the stack contains only your current directory.

## Push a Directory onto the Stack

Use `pushd` to:

1. Add a directory to the top of the stack.
2. Change to that directory.
3. Print the updated stack.

Example:

```bash
$ pwd
/home/john/Work/Projects/Web/src
$ pushd /var/www/html
/var/www/html ~/Work/Projects/Web/src
$ pushd /etc/apache2
/etc/apache2 /var/www/html ~/Work/Projects/Web/src
$ pushd /etc/ssl/certs
/etc/ssl/certs /etc/apache2 /var/www/html ~/Work/Projects/Web/src
$ pwd
/etc/ssl/certs
```

## View a Directory Stack

Print the stack using `dirs`:

```bash
$ dirs -p
/etc/ssl/certs
/etc/apache2
/var/www/html
~/Work/Projects/Web/src
```

Or with numbering:

```bash
$ dirs -v
0 /etc/ssl/certs
1 /etc/apache2
2 /var/www/html
3 ~/Work/Projects/Web/src
```

## Pop a Directory from the Stack

Use `popd` to:

1. Remove the top directory from the stack.
2. Change to the new top directory.
3. Print the updated stack.

Example:

```bash
$ popd
/etc/apache2 /var/www/html ~/Work/Projects/Web/src
$ popd
/var/www/html ~/Work/Projects/Web/src
$ popd
~/Work/Projects/Web/src
$ popd
bash: popd: directory stack empty
$ pwd
~/Work/Projects/Web/src
```

## Swap Directories on the Stack

Running `pushd` with no arguments swaps the top two directories:

```bash
$ dirs
/etc/apache2 ~/Work/Projects/Web/src /var/www/html
$ pushd
~/Work/Projects/Web/src /etc/apache2 /var/www/html
$ pushd
/etc/apache2 ~/Work/Projects/Web/src /var/www/html
```

## Turn a Mistaken `cd` into a `pushd`

If you accidentally use `cd`, restore the lost directory:

```bash
$ dirs
~/Work/Projects/Web/src /var/www/html /etc/apache2
$ cd /etc/ssl/certs
$ dirs
/etc/ssl/certs /var/www/html /etc/apache2
```

Fix it with:

```bash
$ pushd -
~/Work/Projects/Web/src /etc/ssl/certs /var/www/html /etc/apache2
$ pushd
/etc/ssl/certs ~/Work/Projects/Web/src /var/www/html /etc/apache2
```

## Go Deeper into the Stack

Use `pushd +N` to shift directories:

```bash
$ dirs
/etc/ssl/certs ~/Work/Projects/Web/src /var/www/html /etc/apache2
$ pushd +1
~/Work/Projects/Web/src /var/www/html /etc/apache2 /etc/ssl/certs
$ pushd +2
/etc/apache2 /etc/ssl/certs ~/Work/Projects/Web/src /var/www/html
```

Or print numbered directories before jumping:

```bash
$ dirs -v
0 /etc/apache2
1 /etc/ssl/certs
2 ~/Work/Projects/Web/src
3 /var/www/html
```

To jump to `/var/www/html`, use:

```bash
$ pushd +3
```

Remove directories with `popd +N`:

```bash
$ dirs
/var/www/html /etc/apache2 /etc/ssl/certs ~/Work/Projects/Web/src
$ popd +1
/var/www/html /etc/ssl/certs ~/Work/Projects/Web/src
$ popd +2
/var/www/html /etc/ssl/certs
```

