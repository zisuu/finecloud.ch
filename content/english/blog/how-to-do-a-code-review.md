---
title: "How To Do a Code Review"
meta_title: "How To Do a Code Review"
description: "This Blog post is my personal summary of Googles code review process and provides a guide on how to do a code review effectively."
date: 2024-03-18T08:44:00
categories: ["DevOps", "Software Development"]
author: "David"
tags: ["code review", "software development", "best practices"]
draft: false
---

This Blog post is my personal summary of [Googles code review process](https://google.github.io/eng-practices/review/).

## Summary of what you should look at

- **Design**: Is the code well-designed and appropriate for your system?
- **Functionality**: Does the code behave as the author likely intended? Is the way the code behaves good for its users?
- **Complexity**: Could the code be made simpler? Would another developer be able to easily understand and use this code when they come across it in the future?
- **Tests**: Does the code have correct and well-designed automated tests?
- **Naming**: Did the developer choose clear names for variables, classes, methods, etc.?
- **Comments**: Are the comments clear and useful?
- **Style**: Does the code follow our style guides?
- **Documentation**: Did the developer also update relevant documentation?

Make sure to review **every line** of code you’ve been asked to review, look at the **context**, make sure you’re **improving code health**, and compliment developers on **good things** that they do.

## The Standard of Code Review

- You need to balance the tradeoff between making progress, by letting a change go into your code base and not decreasing overall code health and quality.
- Does the Pull-Request improve the maintainability, readability, and understandability?
- In general, you should try to approve a Pull-Request (PR) once it is in a state where it definitely improves the overall code health, even if the PR isn’t perfect.
- Reviewers should not require the author to polish every tiny piece of a CL before granting approval. Rather, the reviewer should balance out the need to make forward progress compared to the importance of the changes they are suggesting.
- Instead of seeking perfection, what a reviewer should seek is continuous improvement.
- If you add a comment on a PR to mention something could be better, but it’s optional and not very important, prefix it with something like “Nit: “ to let the author know that it’s just a point of polish that they could choose to ignore.

## Navigate a Pull-Request (PR) in review

### 1. Take a broad view of the change

Look at the PR description and what the PR does in general. Does this change even make sense? If this change shouldn’t have happened in the first place, please respond immediately with an explanation of why the change should not be happening. When you reject a change like this, it’s also a good idea to suggest to the developer what they should have done instead.

### 2. Examine the main parts of the PR

Find the file or files that are the “main” part of this PR. Often, there is one file that has the largest number of logical changes, and it’s the major piece of the PR. Look at these major parts first. This helps give context to all of the smaller parts of the PR, and generally accelerates doing the code review.

### 3. Look through the rest of the PR in an appropriate sequence

Once you’ve confirmed there are no major design problems with the CL as a whole, try to figure out a logical sequence to look through the files while also making sure you don’t miss reviewing any file.

## Speed of Code Reviews

Why is it so important that you send comments out immediately, especially if you see major design problems within a PR?

- Developers often open a PR and then immediately start new work based on that PR (e.g. feature branch) while they wait for review. If there are major design problems in the PR you’re reviewing, they’re also going to have to re-work their later PR. You want to catch them before they’ve done too much extra work on top of the problematic design.
- Major design changes take longer to do than small changes. Developers nearly all have deadlines; in order to make those deadlines and still have quality code in the codebase, the developer needs to start on any major re-work of the PR as soon as possible.

When code reviews are slow, several things happen:

- The velocity of the team as a whole is decreased.
- Developers start to protest the code review process. Most complaints about the code review process are actually resolved by making the process faster.
- Code health can be impacted.

How fast should a code review be?

- If you are not in the middle of a focused task, **you should do a code review shortly after it comes in**.
- **One business day is the maximum time it should take to respond** to a code review request (i.e., first thing the next morning).

**If you are in the middle of a focused task, such as writing code, don’t interrupt yourself to do a code review**. Research has shown that it can take a long time for a developer to get back into a smooth flow of development after being interrupted. So interrupting yourself while coding is actually more expensive to the team than making another developer wait a bit for a code review.

### Approve with Comments

In order to speed up code reviews, there are certain situations in which a reviewer should give the Approval even though they are also leaving unresolved comments on the PR. This is done when either:

- The reviewer is confident that the developer will appropriately address all the reviewer’s remaining comments.
- The remaining changes are minor and don’t have to be done by the developer.

## How to write comments

- Be kind.
- Explain your reasoning (why?).
- Balance giving explicit directions with just pointing out problems and letting the developer decide -> In general it is the developer’s responsibility to fix a PR, not the reviewer’s.
- Encourage developers to simplify code or add code comments instead of just explaining the complexity to you.
